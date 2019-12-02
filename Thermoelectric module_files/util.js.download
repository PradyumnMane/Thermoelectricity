/**
 * Copyright (c) 1998-2014 by COMSOL AB
 * $Revision: $  $Date: $
 */
"use strict";
var COMSOL = COMSOL || {};

/** Log messages to console and also send them to the server for
    logging in its log file if there is a server connection. */
COMSOL.Logger = (function() {
  function Logger() {
  }

  var NONE    = -1;
  var ERROR   = 0;
  var WARNING = 1;
  var INFO    = 2;
  var DEBUG   = 3;

  var serverConnection;
  var logLevel = INFO;
  if (COMSOL.getQueryParams()["debug"] !== undefined)
    logLevel = parseInt(COMSOL.getQueryParams()["debug"]);

  Logger.setServerConnection = function(connection) {
    serverConnection = connection;
    Logger.println("Server connection created");
  }

  Logger.isDebugEnabled = function() {
    return logLevel >= DEBUG;
  }

  Logger.error = function(msg) {
    doLog(ERROR, msg);
  }

  Logger.warning = function(msg) {
    doLog(WARNING, msg);
  }

  Logger.println = function(msg) {
    doLog(INFO, msg);
  }

  Logger.debug = function(msg) {
    doLog(DEBUG, msg);
  }

  function doLog(level, msg) {
    if (level > logLevel)
      return;
    console.log(msg);
    if (serverConnection !== undefined)
      serverConnection.sendCommand({ data: { commandType: "logMsg", level: level,
                                             message: msg } }, true);
  }

  return Logger;
})();

//------------------------------------------------------------

COMSOL.BrowserInfo = (function() {
  function BrowserInfo() {
  }

  /** Return true if WebGL is supported and enabled. */
  BrowserInfo.isWebGLSupported = function() {
    try {
      if (!window.WebGLRenderingContext)
        return false;
      var canvas = document.createElement("canvas");
      if (!canvas.getContext("webgl") && !canvas.getContext("experimental-webgl"))
        return false;
      return true;
    } catch (ex) {
      return false;
    }
  };

  /** Return maximum supported texture size. */
  BrowserInfo.getMaxTextureSize = function(renderer) {
    if (!renderer.getContext)
      return 0;
    var gl = renderer.getContext();
    if (!gl)
      return 0;
    return gl.getParameter(gl.MAX_TEXTURE_SIZE);
  };

  /** Get the device DPI. */
  BrowserInfo.getDPI = function() {
    var body = document.body;
    if (!body)
      return 0;
    var div = document.createElement("div");
    div.style.width = "1in";
    body.appendChild(div);
    var style = window.getComputedStyle(div, null);
    var dpi = style ? style.getPropertyValue("width") : 96;
    body.removeChild(div);
    return Math.round(parseFloat(dpi));
  }

  return BrowserInfo;
})();
