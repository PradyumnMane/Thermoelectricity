/**
 * Copyright (c) 1998-2014 by COMSOL AB
 * $Revision: $  $Date: $
 */
"use strict";
var COMSOL = COMSOL || {};

// ------------------------------------------------------------

/** Defines data to be sent between client and server.
 * Same data format in both directions. */
COMSOL.ServerCommand = function(commandType) {
  this.data = {
    commandType : commandType,
  };
  this.attachments = [];
};

// ------------------------------------------------------------

/** Handle communication with the web server.
 * Collect received messages, create corresponding ServerCommand objects
 * and send them to a commandReceiver function. */
COMSOL.ServerConnection = (function() {

  var relativePathname = function(basePathname, pathname) {
    var temp = basePathname.split('/');
    temp.pop();
    temp.push(pathname);
    return temp.join('/');
  };

  function ServerConnection(commandReceiver, errorHandler) {
    if (!window.WebSocket)
      errorHandler("Your browser does not support websockets.", true);
    if (!window.Float32Array)
      errorHandler("Your browser does not support Float32Array objects.", true);

    var wsProtocol = (window.location.protocol === "https:") ? "wss:" : "ws:";
    var wsPathname = relativePathname(parent.location.pathname, "webclientsocket");
    var wsUrl = wsProtocol + "//" + window.location.host + wsPathname;
    try {
      var webSocket = new WebSocket(wsUrl);
    } catch (ex) {
      console.log(ex);
      errorHandler("Failed to open websocket.", false);
    }
    var isConnected = false;
    var isInitiated = false;
    var commandQueue = [];
    var initCommandQueue = [];
    var conn = this;

    webSocket.binaryType = "arraybuffer";

    webSocket.onopen = function() {
      isConnected = true;
      var q = initCommandQueue;
      for (var i = 0; i < q.length; i++)
        conn.sendCommand(q[i], true);
      initCommandQueue = [];
    };

    webSocket.onmessage = function(event) {
      if (typeof event.data == "string") {
        var cmd = new COMSOL.ServerCommand();
        cmd.data = JSON.parse(event.data);
        commandReceiver(cmd);
      } else {
        var cmd = new COMSOL.ServerCommand();
        cmd.data = { commandType: "nativeCommand" };
        cmd.attachments.push(event.data);
        commandReceiver(cmd);
      }
    };

    webSocket.onerror = function(error) {
      console.log("WebSocket error: " + error);
    };

    webSocket.onclose = function() {
      isInitiated = false;
      isConnected = false;
      console.log("WebSocket closed");
    }

    this.arrayBufferSupported = false;

    /** Send a command to the server. */
    this.sendCommand = function(command, initCmd) {
      if (isInitiated || (initCmd && isConnected)) {
        if (command.data) {
          webSocket.send(JSON.stringify(command.data));
        }
        if (command.attachments) {
          try {
            var nAtt = command.attachments.length;
            for (var i = 0; i < nAtt; i++)
              webSocket.send(command.attachments[i]);
            this.arrayBufferSupported = true;
          } catch (ex) {
            if (!this.arrayBufferSupported) {
              console.log(ex);
              errorHandler("Your browser does not support WebSocket ArrayBuffer data.", true);
            } else
              throw ex;
          }
        }
      } else if(initCmd){
        initCommandQueue.push(command);
      } else {
        commandQueue.push(command);
      }
    };

    /** Set the connection to initiated and send the buffered commands */
    this.setInitiated = function() {
      if(isConnected) {
        isInitiated = true;
        var q = commandQueue;
        for (var i = 0; i < q.length; i++)
          conn.sendCommand(q[i], false);
        commandQueue = [];
      } else {
        console.log("Tried to initiate when not connected.");
      }
    }

    /** Compute target ID */
    this.getTargetId = function() {
      if (this.targetId === undefined) {
        this.targetId = 0;
        var params = COMSOL.getQueryParams();
        if ("targetid" in params)
          this.targetId = parseInt(params["targetid"]);
        else
          this.targetId = 1;
      }
      return this.targetId;
    }

    conn.sendCommand({data: { commandType: "targetId",
                              targetId: conn.getTargetId() }}, true);

    /** Return true if WebGL 3D rendering should be used. */
    this.useWebGL = function() {
      var params = COMSOL.getQueryParams();
      if (params["3drend"] == "sw")
        return false;
      if (params["webglsupported"] != "1" && // Avoids unnecessary check in tests
          !COMSOL.BrowserInfo.isWebGLSupported())
        return false;
      return true;
    };
  }

  return ServerConnection;
})();

// ------------------------------------------------------------

/** Top-level object for handling the 3D rendering. */
COMSOL.WebRenderer = (function() {

  function WebRenderer(renderTarget) {
    var sceneManager = new COMSOL.SceneManager();
    function commandReceiver(data) {
      sceneManager.handleCommand(data);
    }
    function errorHandler(msg, suggestUpgrade) {
      var w, h;
      if (window.innerWidth) {
        w = window.innerWidth;
        h = window.innerHeight;
      } else if (document.compatMode == "CSS1Compat") {
        w = window.document.documentElement.clientWidth;
        h = window.document.documentElement.clientHeight;
      } else {
        w = window.document.clientWidth;
        h = window.document.clientHeight;
      }
      var padding = 10;
      var border = 1;
      var margin = 1;
      var tot = (padding + border + margin) * 2;
      renderTarget.style.padding = padding.toString() + "px";
      renderTarget.style.border = border.toString() + "px";
      renderTarget.style.margin = margin.toString() + "px";
      renderTarget.style.width = (w - tot).toString() + "px";
      renderTarget.style.height = (h - tot).toString() + "px";
      if (suggestUpgrade) {
        var upgradeMsg = "Please upgrade your browser.";
        msg = msg + "<br><br>" + upgradeMsg;
      }
      renderTarget.innerHTML = msg;
      throw msg;
    }
    var serverConnection = new COMSOL.ServerConnection(commandReceiver,
                                                       errorHandler);
    COMSOL.Logger.setServerConnection(serverConnection);

    /** Initialize rendering functionality. */
    this.initialize = function() {
      function commandSender(command, initCmd) {
        serverConnection.sendCommand(command, initCmd);
      }
      function setInitiatedCallback() {
        serverConnection.setInitiated();
      }
      var w = window.innerWidth;
      var h = window.innerHeight;
      var targetId = serverConnection.getTargetId();
      var useWebGL = serverConnection.useWebGL();
      sceneManager.initialize(renderTarget, setInitiatedCallback, commandSender,
                              useWebGL, targetId, w, h);
      reportWindowSize(w, h);
    };

    var prevWidth, prevHeight;

    /** Handle window resize. */
    this.onResize = function() {
      var w = window.innerWidth;
      var h = window.innerHeight;
      if (w !== prevWidth || h !== prevHeight) {
        sceneManager.onResize(w, h);
        reportWindowSize(w, h);
        prevWidth = w;
        prevHeight = h;
      }
    };

    function reportWindowSize(w, h) {
      serverConnection.sendCommand({data: { commandType: "windowSize",
                                            width : w,
                                            height : h }}, false);
    }
  }

  return WebRenderer;
})();

// ------------------------------------------------------------

/** Interacts with three.js to create the 3D scene. */
COMSOL.SceneManager = (function() {

  function SceneManager() {
    /** Function that sends a command to the server. */
    var sendCommand;

    var mouseCtrl, nativeScene;

    /** Initialize graphics system.
     * @param renderTarget DOM element where graphics is rendered.
     * @param setInitiatedCallback Function to call when set as initiated from server.
     * @param commandSender Function to send commands to the server.
     * @param useWebGL True to use WebGL 3D graphics, false to use 2D images.
     */
    this.initialize = function(renderTarget, setInitiatedCallback, commandSender,
                               useWebGL, targetId, w, h) {
      sendCommand = commandSender;

      var canvas3D = document.createElement("canvas");
      renderTarget.appendChild(canvas3D);

      var context2D;
      if (useWebGL) {
        var canvas2D = document.createElement("canvas");
        canvas2D.style.position = "absolute";
        canvas2D.style.left = "0px";
        canvas2D.style.top = "0px";
        canvas2D.width = w;
        canvas2D.height = h;
        renderTarget.appendChild(canvas2D);
        context2D = canvas2D.getContext("2d");
      } else {
        context2D = canvas3D.getContext("2d");
      }
      canvas3D.style.position = "absolute";
      canvas3D.style.left = "0px";
      canvas3D.style.top = "0px";

      nativeScene = new COMSOL.NativeScene(canvas3D, useWebGL, context2D, setInitiatedCallback,
                                           sendCommand, targetId, w, h);

      var pendingRedraw = false;

      /** Mouse movement callback function. */
      function notify(zoomPanInfo, params) {
        if (params && params.stop) {
          nativeScene.setMouseAction(false, zoomPanInfo);
        } else {
          nativeScene.setMouseAction(true, zoomPanInfo);
          if (!pendingRedraw) {
            pendingRedraw = true;
            requestAnimationFrame(function() {
              pendingRedraw = false;
              nativeScene.renderScene();
            });
          }
        }
      }

      function reportMouseEvent(eventType, event, wheelDelta) {
        nativeScene.reportMouseEvent(eventType, event, wheelDelta);
      }

      mouseCtrl = new COMSOL.MouseControl(nativeScene.get3DCamera(), document, notify, reportMouseEvent);
      nativeScene.setMouseControl(mouseCtrl);
      nativeScene.renderScene();
    };

    /** Handle a command received from the server. */
    this.handleCommand = function(command) {
      var nAttach = command.attachments.length;
      if (command.data.commandType == "nativeCommand") {
        for (var i = 0; i < nAttach; i++)
          nativeScene.handleCommand(command.attachments[i]);
      }
    };

    /** Handle window resize. */
    this.onResize = function(w, h) {
      nativeScene.onResize(w, h);
    };
  }

  return SceneManager;
})();

// ------------------------------------------------------------

/** Keep track of zoom/pan info during mouse gestures in 2D mode. */
COMSOL.ZoomPanInfo = (function() {
  function ZoomPanInfo() {
    this.reset();
  }

  /** Reset object to default state. */
  ZoomPanInfo.prototype.reset = function() {
    this.startX = 0;  // Gesture start position, between 0 (left) and 1 (right)
    this.startY = 0;  // Gesture start position, between 0 (bottom) and 1 (top)
    this.zoom  = 1;   // Zoom factor, >1 when zooming in
    this.panX = 0;    // Pan distance in relative window coordinates
    this.panY = 0;
    this.overlayBoxEndX = -1; // Zoom box end position, between 0 (left) and 1 (right), or -1 when not active
    this.overlayBoxEndY = -1; // Zoom box end position, between 0 (bottom) and 1 (top), or -1 when not active
  };

  /** Set startX and startY. */
  ZoomPanInfo.prototype.setStart = function(startX, startY) {
    this.reset();
    this.startX = startX / window.innerWidth;
    this.startY = 1 - startY / window.innerHeight;
  };

  /** Apply pan transform. */
  ZoomPanInfo.prototype.applyPan = function(dx, dy) {
    this.panX +=  dx / window.innerWidth;
    this.panY += -dy / window.innerHeight;
    if (this.zoom != 1) {
      this.startX -= this.panX / (this.zoom - 1);
      this.startY -= this.panY / (this.zoom - 1);
      this.panX = 0;
      this.panY = 0;
    }

    this.overlayBoxEndX = this.overlayBoxEndY = -1;
  };

  /** Apply zoom transform. */
  ZoomPanInfo.prototype.applyZoom = function(x, y, scale) {
    var sx = this.startX;
    var z = this.zoom;
    var sx2 = x / window.innerWidth;
    var z2 = 1 / scale;
    this.startX = (z*z2==1) ? 0.5 : (sx2*(1-z2)+sx*(1-z)*z2)/(1-z*z2);
    this.zoom = z * z2;
    var sy = this.startY;
    var sy2 = 1 - y / window.innerHeight;
    this.startY = (z*z2==1) ? 0.5 : (sy2*(1-z2)+sy*(1-z)*z2)/(1-z*z2);

    this.panX = this.panY = 0;
    this.overlayBoxEndX = this.overlayBoxEndY = -1;
  };

  /** Define second corner of zoom box. */
  ZoomPanInfo.prototype.setOverlayBoxEnd = function(endX, endY) {
    this.overlayBoxEndX = endX / window.innerWidth;
    this.overlayBoxEndY = 1 - endY / window.innerHeight;

    this.zoom = 1;
    this.panX = this.panY = 0;
  };

  return ZoomPanInfo;
})();

// ------------------------------------------------------------

/** Modify camera transforms based on mouse movements. */
COMSOL.MouseControl = (function() {

  var CameraOrbitSceneMouseAction    = 1;
  var CameraRotateSceneMouseAction   = 2;
  var CameraMoveSceneMouseAction     = 3;
  var CameraDollySceneMouseAction    = 4;
  var CameraRotateUpRightMouseAction = 5;
  var CameraRotateAtMouseAction      = 6;
  var CameraMoveMouseAction          = 7;
  var CameraDollyMouseAction         = 8;
  var CameraPanMouseAction           = 9;
  var CameraZoomMouseAction          = 10;
  var CameraNoMouseAction            = 11;
  var CameraZoomBoxAction            = 100; // Interactive zoom box

  /** Constructor.
   * @param camera            The ComsolCamera object to control.
   * @param domElement        The DOM element containing the 3D graphics.
   * @param notify            Function to be called when camera has been changed.
   * @param reportMouseEvent  Function to be called to report mouse events.
   */
  function MouseControl(camera, domElement, notify, reportMouseEvent) {
    this.camera = camera;
    this.overlayBoxEnabled = false;
    var self = this;

    var currAction = CameraNoMouseAction;

    var mouseClickStartX = -1;
    var mouseClickStartY = -1;

      
    var startX = -1, startY = -1;
    var lastX = -1, lastY = -1;

    // True if camera settings have been modified since they were last sent to the server
    var modified = false;

    var zoomPanInfo = new COMSOL.ZoomPanInfo();

    // Accumulated mouse wheel value.
    var mouseWheelAccum = 0;

    var buttonDown = false;

    document.body.style.touchAction = "none";
    document.body.style.msTouchAction = "none";
    domElement.addEventListener("contextmenu", onContextMenu, false);
    domElement.addEventListener("mousedown", onMouseDown, false);
    domElement.addEventListener("mousemove", onMouseMove, false);
    domElement.addEventListener("mousewheel", onMouseWheel, false);
    domElement.addEventListener("keydown", onKeyDown, false);
    domElement.addEventListener("keyup", onKeyUp, false);
    domElement.addEventListener("click", onMouseClick, false);
    domElement.addEventListener("DOMMouseScroll", onDOMMouseScroll, false);
    domElement.addEventListener("touchstart", onTouchStart, false);
    domElement.addEventListener("touchmove", onTouchMove, false);
    domElement.addEventListener("touchend", onTouchEnd, false);
    if (window.PointerEvent) {
      domElement.addEventListener("pointerdown", onPointerDown, false);
      domElement.addEventListener("pointermove", onPointerMove, false);
      domElement.addEventListener("pointerup", onPointerUp, false);
      domElement.addEventListener("pointercancel", onPointerUp, false);
    }

    function onContextMenu(event) {
      event.preventDefault();
      window.focus();
    }

    function onMouseDown(event) {
      event.preventDefault();
      event.stopPropagation();
      window.focus();
      mouseClickStartX = event.clientX;
      mouseClickStartY = event.clientY;
      reportMouseEvent(1, event, 0);
      if ((currAction == CameraZoomBoxAction) && (event.button == 2)) {
        // Right mouse button cancels zoom box action
        currAction = CameraNoMouseAction;
        modified = false;
        zoomPanInfo.reset();
        notify(zoomPanInfo, {stop: true});
        return;
      }
      if (currAction != CameraNoMouseAction)
        return;
      buttonDown = true;
      document.addEventListener("mouseup", onMouseUp, false);
      startAction(event.button, event.altKey, event.ctrlKey, event.clientX, event.clientY);

      mouseWheelAccum = 0;
    }

    function onMouseUp(event) {
      event.preventDefault();
      event.stopPropagation();
      window.focus();

      buttonDown = false;
      document.removeEventListener("mouseup", onMouseUp, false);
      applyAction(event.clientX, event.clientY);
      stopAction();

      reportMouseEvent(2, event, 0);
      mouseWheelAccum = 0;
    }

    function onMouseClick(event) {
      event.preventDefault();
      event.stopPropagation();
      window.focus();
      if (mouseClickStartX != -1 && mouseClickStartY != -1 &&
          mouseClickStartX==event.clientX && mouseClickStartY==event.clientY) {
          reportMouseEvent(3, event, 0);
          mouseClickStartX = mouseClickStartY = -1;
      }

      mouseWheelAccum = 0;
    }

    function onMouseMove(event) {
      event.preventDefault();
      event.stopPropagation();
      if (currAction == CameraNoMouseAction) {
        if(startX >= 0 && startY >= 0) {
          if(Math.abs(startX - event.clientX) > 10 || Math.abs(startY - event.clientY) > 10) {
            mouseWheelAccum = 0;
            startX = -1;
            startY = -1;
          }
        }
        reportMouseEvent(5, event, 0);
      } else {
        applyAction(event.clientX, event.clientY);
        mouseWheelAccum = 0;
      }
      lastX = event.clientX;
      lastY = event.clientY;
    }

    function onMouseWheel(event) {
      event.preventDefault();
      event.stopPropagation();
      window.focus();
      if(currAction == CameraNoMouseAction) {
        if(startX < 0 && startY < 0) {
          startX = event.clientX;
          startY = event.clientY;
          mouseWheelAccum = 0;
        }
        mouseWheelAccum += event.wheelDelta;
        while(mouseWheelAccum >= 120) {
          reportMouseEvent(4, event, 1);
          mouseWheelAccum -= 120;
        }
        while(mouseWheelAccum <= -120) {
          reportMouseEvent(4, event, -1);
          mouseWheelAccum += 120;
        }
      }
    }

    function onKeyDown(event) {
      event.preventDefault();
      event.stopPropagation();
      event.clientX = lastX;
      event.clientY = lastY;
      switch(event.keyCode) {
        case 38: // UP
          reportMouseEvent(4, event, 1);
          mouseWheelAccum = 0;
          break;
        case 40: // DOWN
          reportMouseEvent(4, event, -1);
          mouseWheelAccum = 0;
          break;
      }
    }

    function onKeyUp(event) {
      event.preventDefault();
      event.stopPropagation();
    }

    function onDOMMouseScroll(event) {
      event.preventDefault();
      event.stopPropagation();
      window.focus();
      if(event.detail > 0) {
        reportMouseEvent(4, event, -1);
      } else {
        reportMouseEvent(4, event, 1);
      }
    }


    // Keep track of active pointers
    var touches = [];

    function onPointerDown(event) {
      if (event.pointerType === "mouse")
        return;
      event.preventDefault();
      event.stopPropagation();
      window.focus();

      event.target.setPointerCapture(event.pointerId);

      var found = false;
      for (var i = 0; i < touches.length; i++) {
        if (touches[i].pointerId === event.pointerId) {
          found = true;
          break;
        }
      }
      if (!found) {
        touches.push({pointerId: event.pointerId,
                      pageX: event.pageX,
                      pageY: event.pageY});
        onTouchStart({preventDefault: function() {},
                      stopPropagation: function() {},
                      touches: touches});
      }
    }

    function onPointerMove(event) {
      if (event.pointerType === "mouse")
        return;
      event.preventDefault();
      event.stopPropagation();

      for (var i = 0; i < touches.length; i++) {
        if (touches[i].pointerId === event.pointerId) {
          touches[i].pageX = event.pageX;
          touches[i].pageY = event.pageY;
          onTouchMove({preventDefault: function() {},
                        stopPropagation: function() {},
                        touches: touches});
          break;
        }
      }
    }

    function onPointerUp(event) {
      if (event.pointerType === "mouse")
        return;
      event.preventDefault();
      event.stopPropagation();
      window.focus();

      for (var i = touches.length-1; i >= 0; i--)
        if (touches[i].pointerId === event.pointerId)
          touches.splice(i, 1);
      if (touches.length === 0)
        onTouchEnd();
    }


    // Number of fingers on screen
    var currTouchLen = 0;

    // For two-finger gestures
    var currPinchAvg = new THREE.Vector2();
    var currPinchDiff = new THREE.Vector2();

    function onTouchStart(event) {
      event.preventDefault();
      event.stopPropagation();
      window.focus();
      var t = event.touches;
      var len = t.length;
      var x = len > 0 ? t[0].pageX : 0;
      var y = len > 0 ? t[0].pageY : 0;
      currTouchLen = len;
      if (len == 2) {
        currPinchAvg.set((t[0].pageX + t[1].pageX) / 2,
                         (t[0].pageY + t[1].pageY) / 2);
        currPinchDiff.set(t[1].pageX - t[0].pageX,
                          t[1].pageY - t[0].pageY);
        zoomPanInfo.setStart(currPinchAvg.x, currPinchAvg.y);
      } else {
        startAction(len-1, false, false, x, y);
      }
    }

    function onTouchMove(event) {
      event.preventDefault();
      event.stopPropagation();
      var t = event.touches;
      if (t.length != currTouchLen)
        return;
      switch (t.length) {
      case 1:
      case 3:
        applyAction(t[0].pageX, t[0].pageY);
        lastX = t[0].pageX;
        lastY = t[0].pageY;
        break;
      case 2:
        if (currPinchDiff.length() > 0) {
          var xMid = (t[0].pageX + t[1].pageX) / 2;
          var yMid = (t[0].pageY + t[1].pageY) / 2;
          var dx = t[1].pageX - t[0].pageX;
          var dy = t[1].pageY - t[0].pageY;
          var newPinch = Math.sqrt(dx*dx + dy*dy);
          var newPinchVec = new THREE.Vector2(dx, dy);
          if (newPinchVec.length() > 0) {
            var scale = currPinchDiff.length() / newPinchVec.length();
            applyZoom(xMid, yMid, scale);

            var currAngle = Math.atan2(currPinchDiff.y, currPinchDiff.x);
            var newAngle = Math.atan2(dy, dx);
            applyRotation(0, 0, currAngle - newAngle);

            var newPinchAvg = new THREE.Vector2((t[0].pageX + t[1].pageX) / 2,
                                                (t[0].pageY + t[1].pageY) / 2);
            applyPan(newPinchAvg.x - currPinchAvg.x,
                     newPinchAvg.y - currPinchAvg.y);

            currPinchAvg = newPinchAvg;
            currPinchDiff = newPinchVec;
          }
        }
        break;
      }
    }

    function onTouchEnd(event) {
      stopAction();
      window.focus();
      currTouchLen = 0;
    }


    /** Start a camera action corresponding to button and modifier key state. */
    function startAction(button, altKey, ctrlKey, x, y) {
      currAction = CameraNoMouseAction;
      switch (button) {
      case 0:
        if (camera.isMode2D()) {
          if (self.overlayBoxEnabled)
            currAction = CameraZoomBoxAction;
        } else {
          if (self.overlayBoxEnabled) {
            currAction = CameraZoomBoxAction;
          } else if (altKey) {
            if (ctrlKey)
              currAction = CameraRotateAtMouseAction;
            else
              currAction = CameraRotateSceneMouseAction;
          } else {
            if (ctrlKey)
              currAction = CameraRotateUpRightMouseAction;
            else
              currAction = CameraOrbitSceneMouseAction;
          }
        }
        break;
      case 1:
        if (altKey) {
          if (!ctrlKey)
            currAction = CameraDollySceneMouseAction;
        } else {
          if (ctrlKey)
            currAction = CameraDollyMouseAction;
          else
            currAction = CameraZoomMouseAction;
        }
        break;
      case 2:
        if (altKey) {
          if (!ctrlKey)
            currAction = CameraMoveSceneMouseAction;
        } else {
          if (ctrlKey)
            currAction = CameraMoveMouseAction;
          else
            currAction = CameraPanMouseAction;
        }
        break;
      }
      startX = lastX = x;
      startY = lastY = y;
      zoomPanInfo.setStart(startX, startY);
    }

    /** Modify camera according to current mouse action. */
    function applyAction(x, y) {
      switch (currAction) {
      case CameraOrbitSceneMouseAction:
      case CameraRotateAtMouseAction:      // COMSOL behavior not implemented
      case CameraRotateSceneMouseAction:   // COMSOL behavior not implemented
      case CameraRotateUpRightMouseAction: // COMSOL behavior not implemented
        var dx = x - lastX;
        var dy = y - lastY;
        applyRotation(-dy * 0.01, -dx * 0.01, 0);
        break;
      case CameraZoomMouseAction:
        var delta = (x - lastX) + (y - lastY);
        var scale = 1 + delta / 200;
        applyZoom(startX, startY, scale);
        break;
      case CameraDollyMouseAction:
      case CameraDollySceneMouseAction:    // COMSOL behavior not implemented
        if (!camera.isMode2D()) {
          var delta = ((x - lastX) + (y - lastY)) * 0.02;
          applyTranslation(0, 0, delta);
        }
        break;
      case CameraPanMouseAction:
        if (x != lastX || y != lastY) {
          var dx = x - lastX;
          var dy = y - lastY;
          applyPan(dx, dy);
        }
        break;
      case CameraMoveMouseAction:
      case CameraMoveSceneMouseAction:     // COMSOL behavior not implemented
        var dx = x - lastX;
        var dy = lastY - y;
        if (camera.isMode2D()) {
          applyPan(dx, -dy);
        } else {
          var dist = camera.rotationPoint.clone().sub(camera.position).length();
          var height = window.innerHeight;
          var panSpeed;
          if (camera.isPerspectiveProjection())
            panSpeed = 2 * Math.tan(camera.fov / 2) / height * dist;
          else
            panSpeed = camera.orthoScale / (camera.aspect * height);
          applyTranslation(-dx * panSpeed, -dy * panSpeed, 0);
        }
        break;
      case CameraZoomBoxAction:
        zoomPanInfo.setOverlayBoxEnd(x, y);
        modified = true;
        notify(zoomPanInfo);
        break;
      }
    }

    function stopAction() {
      currAction = CameraNoMouseAction;
      if (modified) {
        notify(zoomPanInfo, {stop: true});
      }
      modified = false;
      zoomPanInfo.reset();
      startX = -1;
      startY = -1;
    }

    function applyPan(dx, dy) {
      if (camera.isMode2D()) {
        zoomPanInfo.applyPan(dx, dy);
      } else {
        camera.nearRelDeltaX +=  dx / window.innerWidth;
        camera.nearRelDeltaY += -dy / window.innerHeight;
        camera.updateProjectionMatrix();
      }
      modified = true;
      notify(zoomPanInfo);
    }

    function applyZoom(x, y, scale) {
      if (scale == 1)
        return;
      if (camera.isMode2D()) {
        zoomPanInfo.applyZoom(x, y, scale);
      } else {
        scale = Math.min(Math.max(scale, 0.5), 1.5);
        camera.applyZoom3D(x, y, scale);
      }
      modified = true;
      notify(zoomPanInfo);
    }

    function applyRotation(rightAxisRot, upAxisRot, atAxisRot) {
      if (camera.isMode2D())
          return;
      if (!rightAxisRot && !upAxisRot && !atAxisRot)
        return;
      camera.updateMatrix();
      var camPos   = (new THREE.Vector4(0,0,0,1)).applyMatrix4(camera.matrix);
      var camUp    = (new THREE.Vector4(0,1,0,0)).applyMatrix4(camera.matrix);
      var camAt    = (new THREE.Vector4(0,0,1,0)).applyMatrix4(camera.matrix);

      var at = (new THREE.Vector3()).subVectors(camera.rotationPoint, camPos);
      var unitAt = at.clone().normalize();
      var right = (new THREE.Vector3()).crossVectors(at, camUp).normalize();
      var tmp  = (new THREE.Vector3()).crossVectors(right, at).normalize();
      var rot1 = (new THREE.Matrix4()).makeRotationAxis(tmp, upAxisRot);
      var rot2 = (new THREE.Matrix4()).makeRotationAxis(right, rightAxisRot);
      var rot3 = (new THREE.Matrix4()).makeRotationAxis(unitAt, atAxisRot);
      var rot = (new THREE.Matrix4()).multiplyMatrices(rot1, rot2);
      rot = (new THREE.Matrix4()).multiplyMatrices(rot, rot3);

      var posVec = camera.rotationPoint.clone().sub(at.clone().applyMatrix4(rot));
      camera.position.set(posVec.x, posVec.y, posVec.z);
      var upVec4 = camUp.clone().applyMatrix4(rot);
      camera.up.set(upVec4.x, upVec4.y, upVec4.z);
      var len = camera.position.clone().sub(camera.target).length();
      camera.target = camera.position.clone().sub(camAt.clone().multiplyScalar(len).applyMatrix4(rot));
      camera.lookAt(camera.target);
      modified = true;
      notify(zoomPanInfo);
    }

    function applyTranslation(rightDelta, upDelta, atDelta) {
      if (!rightDelta && !upDelta && !atDelta)
        return;
      var right = (new THREE.Vector4(1,0,0,0)).applyMatrix4(camera.matrix);
      var up = (new THREE.Vector4(0,1,0,0)).applyMatrix4(camera.matrix);
      var at = (new THREE.Vector4(0,0,1,0)).applyMatrix4(camera.matrix);
      var d = new THREE.Vector3(right.x*rightDelta + up.x*upDelta + at.x*atDelta,
                                right.y*rightDelta + up.y*upDelta + at.y*atDelta,
                                right.z*rightDelta + up.z*upDelta + at.z*atDelta);
      camera.position.add(d);
      camera.target.add(d);
      modified = true;
      notify(zoomPanInfo);
    }
  }

  /** Enable/disable zoom box mode. */
  MouseControl.prototype.setOverlayBoxMode = function(overlayBoxEnabled) {
    this.overlayBoxEnabled = overlayBoxEnabled;
  };

  /** Return the associated ComsolCamera object. */
  MouseControl.prototype.getCamera = function() {
    return this.camera;
  }

  return MouseControl;
})();

// ------------------------------------------------------------

/** Like THREE.PerspectiveCamera but also supports relDeltaX, relDeltaY
 * to implement camera panning. */
COMSOL.ComsolCamera = (function() {
  function ComsolCamera(fov, aspect, near, far, relDeltaX, relDeltaY) {
    THREE.Camera.call(this);

    this.isMode3D = true;       // True when in 3D mode, false when in 2D/1D mode.

    // 3D
    this.projectionType = ComsolCamera.PerspectiveProjection;
    this.fov = fov;
    this.orthoScale = 1.0;
    this.aspect = aspect;
    this.near = near;
    this.far = far;
    this.nearRelDeltaX = relDeltaX || 0;
    this.nearRelDeltaY = relDeltaY || 0;
    this.target = new THREE.Vector3();
    this.rotationPoint = new THREE.Vector3();

    // 2D, pixel coordinates defining draw area
    this.drawMinX = 0;
    this.drawMaxX = 1;
    this.drawMinY = 0;
    this.drawMaxY = 1;
    // 2D, axis limits
    this.xAxisMin = 0;
    this.xAxisMax = 1;
    this.yAxisMin = 0;
    this.yAxisMax = 1;

    this.updateProjectionMatrix();
  }

  ComsolCamera.prototype = Object.create(THREE.Camera.prototype);
  ComsolCamera.prototype.constructor = ComsolCamera;

  ComsolCamera.OrthographicProjection = 0;
  ComsolCamera.PerspectiveProjection = 1;

  /** Return true if camera is in 2D mode. */
  ComsolCamera.prototype.isMode2D = function() {
    return !this.isMode3D;
  }

  /** Return true if camera is in perspective projection mode. */
  ComsolCamera.prototype.isPerspectiveProjection = function() {
    return this.projectionType == ComsolCamera.PerspectiveProjection;
  }

  ComsolCamera.prototype.updateProjectionMatrix = function() {
    if (this.isMode2D())
      return;
    var aspect = this.aspect;
    var near = this.near;
    var far = this.far;
    var nearRelDeltaX = this.nearRelDeltaX;
    var nearRelDeltaY = this.nearRelDeltaY;

    if (this.isPerspectiveProjection()) {
      var fov = this.fov;
      var nearHeight = near * 2 * Math.tan(fov / 2);
      var nearWidth = nearHeight * aspect;

      var m11 = 2 * near / nearWidth;
      var m13 = -2 * nearRelDeltaX;
      var m22 = 2 * near / nearHeight;
      var m23 = -2 * nearRelDeltaY;
      var m33 = -(far + near) / (far - near);
      var m34 = -2 * far * near / (far - near);

      var m = this.projectionMatrix.elements;
      m[0] = m11;  m[4] = 0;    m[8]  = m13;  m[12] = 0;
      m[1] = 0;    m[5] = m22;  m[9]  = m23;  m[13] = 0;
      m[2] = 0;    m[6] = 0;    m[10] = m33;  m[14] = m34;
      m[3] = 0;    m[7] = 0;    m[11] = -1;   m[15] = 0;
    } else {
      var nearWidth = this.orthoScale;
      var nearHeight = nearWidth / this.aspect;

      var m11 = 2 / nearWidth;
      var m14 = 2 * nearRelDeltaX;
      var m22 = 2 / nearHeight;
      var m24 = 2 * nearRelDeltaY;
      var m33 = -2 / (far - near);
      var m34 = -(far + near) / (far - near);
      var m = this.projectionMatrix.elements;
      m[0] = m11;  m[4] = 0;    m[8]  = 0;    m[12] = m14;
      m[1] = 0;    m[5] = m22;  m[9]  = 0;    m[13] = m24;
      m[2] = 0;    m[6] = 0;    m[10] = m33;  m[14] = m34;
      m[3] = 0;    m[7] = 0;    m[11] = 0;    m[15] = 1.0;
    }
  };

  /** Set camera mode to 3D or 2D/1D. */
  ComsolCamera.prototype.setMode = function(mode3D) {
    this.isMode3D = mode3D;
  };

  /** Set camera in 3D mode. Sets all camera parameters except aspect/near/far. */
  ComsolCamera.prototype.setView3D = function(camDef) {
    this.projectionType = camDef.projectionType;
    this.nearRelDeltaX = camDef.viewOffsetX;
    this.nearRelDeltaY = camDef.viewOffsetY;
    this.rotationPoint.set(camDef.rotCenterX, camDef.rotCenterY, camDef.rotCenterZ);
    this.position.set(camDef.positionX, camDef.positionY, camDef.positionZ);

    if (this.projectionType == ComsolCamera.PerspectiveProjection) {
      this.fov = camDef.zoom * 2;
    } else if (this.projectionType == ComsolCamera.OrthographicProjection) {
      this.orthoScale = camDef.zoom;
    } else {
      throw "Invalid projection type: " + this.projectionType;
    }

    this.up.set(camDef.upX, camDef.upY, camDef.upZ);
    this.target.set(camDef.targetX, camDef.targetY, camDef.targetZ);
    this.lookAt(this.target);

    this.updateMatrix();
    this.updateProjectionMatrix();
  };

  /** Set camera in 2D mode. */
  ComsolCamera.prototype.setView2D = function(camDef) {
    this.drawMinX = camDef.drawMinX;
    this.drawMaxX = camDef.drawMaxX;
    this.drawMinY = camDef.drawMinY;
    this.drawMaxY = camDef.drawMaxY;
    this.xAxisMin = camDef.xAxisMin;
    this.xAxisMax = camDef.xAxisMax;
    this.yAxisMin = camDef.yAxisMin;
    this.yAxisMax = camDef.yAxisMax;
  };

  /** Update 2D/3D view based on zoom/pan information. */
  ComsolCamera.prototype.applyZoomPanInfo = function(zoomPanInfo) {
    var w = window.innerWidth;
    var h = window.innerHeight;
    var reportSettings = true;

    if (this.isMode3D) {
      if ((zoomPanInfo.overlayBoxEndX != -1) && (zoomPanInfo.overlayBoxEndY != -1)) { // Zoom box
        reportSettings = false;
      }
//        var boxXMin = Math.min(zoomPanInfo.startX, zoomPanInfo.overlayBoxEndX) * w;
//        var boxXMax = Math.max(zoomPanInfo.startX, zoomPanInfo.overlayBoxEndX) * w;
//        var boxYMin = Math.min(zoomPanInfo.startY, zoomPanInfo.overlayBoxEndY) * h;
//        var boxYMax = Math.max(zoomPanInfo.startY, zoomPanInfo.overlayBoxEndY) * h;
//        var centerX = (boxXMin+boxXMax)/(2*w);
//        var centerY = (boxYMin+boxYMax)/(2*h);
//        var scaleX = (boxXMax - boxXMin) / w;
//        var scaleY = (boxYMax - boxYMin) / h;
//        this.nearRelDeltaX += 0.5 - centerX;
//        this.nearRelDeltaY += 0.5 - centerY;
//        this.applyZoom3D(w/2, h/2, Math.max(scaleX, scaleY));
//      }
    } else {
      if ((zoomPanInfo.overlayBoxEndX != -1) && (zoomPanInfo.overlayBoxEndY != -1)) { // Zoom box
        reportSettings = false;
//        var boxXMin = Math.min(zoomPanInfo.startX, zoomPanInfo.overlayBoxEndX) * w;
//        var boxXMax = Math.max(zoomPanInfo.startX, zoomPanInfo.overlayBoxEndX) * w;
//        var boxYMin = Math.min(zoomPanInfo.startY, zoomPanInfo.overlayBoxEndY) * h;
//        var boxYMax = Math.max(zoomPanInfo.startY, zoomPanInfo.overlayBoxEndY) * h;
//        var xMin = this.xAxisMin + (boxXMin - this.drawMinX) / (this.drawMaxX - this.drawMinX) *
//                                   (this.xAxisMax - this.xAxisMin);
//        var xMax = this.xAxisMin + (boxXMax - this.drawMinX) / (this.drawMaxX - this.drawMinX) *
//                                   (this.xAxisMax - this.xAxisMin);
//        var yMin = this.yAxisMin + (boxYMin - this.drawMinY) / (this.drawMaxY - this.drawMinY) *
//                                   (this.yAxisMax - this.yAxisMin);
//        var yMax = this.yAxisMin + (boxYMax - this.drawMinY) / (this.drawMaxY - this.drawMinY) *
//                                   (this.yAxisMax - this.yAxisMin);
//        this.xAxisMin = xMin;
//        this.xAxisMax = xMax;
//        this.yAxisMin = yMin;
//        this.yAxisMax = yMax;
      } else if ((zoomPanInfo.panX != 0) || (zoomPanInfo.panY != 0)) { // Pan
        var dx = zoomPanInfo.panX * (this.xAxisMax - this.xAxisMin) * w / (this.drawMaxX - this.drawMinX);
        var dy = zoomPanInfo.panY * (this.yAxisMax - this.yAxisMin) * h / (this.drawMaxY - this.drawMinY);
        this.xAxisMin -= dx;
        this.xAxisMax -= dx;
        this.yAxisMin -= dy;
        this.yAxisMax -= dy;
      } else { // Zoom
        var xc = this.xAxisMin + (zoomPanInfo.startX * w - this.drawMinX) / (this.drawMaxX - this.drawMinX) *
                                 (this.xAxisMax - this.xAxisMin);
        var yc = this.yAxisMin + (zoomPanInfo.startY * h - this.drawMinY) / (this.drawMaxY - this.drawMinY) *
                                 (this.yAxisMax - this.yAxisMin);
        var z = zoomPanInfo.zoom;
        var xMin = xc + (this.xAxisMin - xc) / z;
        var xMax = xc + (this.xAxisMax - xc) / z;
        var yMin = yc + (this.yAxisMin - yc) / z;
        var yMax = yc + (this.yAxisMax - yc) / z;
        this.xAxisMin = xMin;
        this.xAxisMax = xMax;
        this.yAxisMin = yMin;
        this.yAxisMax = yMax;
      }
    }
    return reportSettings;
  };

  /** Scale view around a point given in pixel coordinates. */
  ComsolCamera.prototype.applyZoom3D = function(x, y, scale) {
    var w = window.innerWidth;
    var h = window.innerHeight;
    var centerX = x / w;
    var centerY = (h - y) / h;

    this.nearRelDeltaX += (centerX - 0.5 - this.nearRelDeltaX) * (1 - 1/scale);
    this.nearRelDeltaY += (centerY - 0.5 - this.nearRelDeltaY) * (1 - 1/scale);
    if (this.isPerspectiveProjection())
      this.fov = Math.atan(Math.tan(this.fov / 2) * scale) * 2;
    else
      this.orthoScale *= scale;
    this.updateProjectionMatrix();
  };

  return ComsolCamera;
})();

// ------------------------------------------------------------

/** Expand the bounding box to include a point. */
THREE.Box3.prototype.expandByPoint = function(point) {
  this.min.min(point);
  this.max.max(point);
  return this;
};
