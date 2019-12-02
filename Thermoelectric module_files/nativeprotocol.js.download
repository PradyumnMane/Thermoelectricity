/**
 * Copyright (c) 1998-2014 by COMSOL AB
 * $Revision: $  $Date: $
 */
"use strict";
var COMSOL = COMSOL || {};

// ------------------------------------------------------------

COMSOL.NativeProtocol = (function() {

  function NativeProtocol() {
  }

  // Data types
  var UINT32  = 0;
  var UINT16  = 1;
  var UINT8   = 2;
  var FLOAT32 = 3;
  var ARRAY   = 4;

  // Message types
  NativeProtocol.mt = {};
  NativeProtocol.mt.IMAGE_DATA = 0x0001;

  NativeProtocol.mt.SCENE_FINISHED = 0x0101;
  NativeProtocol.mt.SET_RENDERER   = 0x0102;
  NativeProtocol.mt.OVERLAY_BOX_MODE  = 0x0103;

  NativeProtocol.mt.TEXTURE_2D     = 0x201;
  NativeProtocol.mt.SUBTEXTURE_2D  = 0x202;
  NativeProtocol.mt.DELETE_TEXTURE = 0x203;

  NativeProtocol.mt.CREATE_VERTEX_BUFFER = 0x0301;
  NativeProtocol.mt.FILL_VERTEX_BUFFER   = 0x0302;
  NativeProtocol.mt.DELETE_VERTEX_BUFFER = 0x0303;

  NativeProtocol.mt.CREATE_INDEX_BUFFER = 0x0401;
  NativeProtocol.mt.FILL_INDEX_BUFFER   = 0x0402;
  NativeProtocol.mt.DELETE_INDEX_BUFFER = 0x0403;

  NativeProtocol.mt.DEFINE_SEQUENCE       = 0x0501;
  NativeProtocol.mt.DELETE_SEQUENCE       = 0x0502;
  NativeProtocol.mt.DEFINE_SEQUENCE_ORDER = 0x0503;
  NativeProtocol.mt.DEFINE_SUB_SEQUENCE   = 0x0504;

  NativeProtocol.mt.SETUP_CAMERA_3D       = 0x0702;
  NativeProtocol.mt.SETUP_CAMERA_2D       = 0x0703;
  NativeProtocol.mt.USE_LIGHT             = 0x0710;

  NativeProtocol.mt.COORDINATE_BOX        = 0x0801;
  NativeProtocol.mt.DEFINE_BACKGROUND     = 0x0802;

  NativeProtocol.mt.CONNECTION_INITIATED  = 0x9000;

  // Message format definitions
  NativeProtocol.mf = {};
  NativeProtocol.mf.HEADER = [ UINT16, "target",
                               UINT16, "msgType",
                               ARRAY,  "msgData" ];

  NativeProtocol.mf.IMAGE_DATA = [ UINT16, "layer",
                                   ARRAY, "data" ];

  NativeProtocol.mf.SCENE_FINISHED = [ ];

  NativeProtocol.mf.SET_RENDERER = [ UINT16, "renderer" ];

  NativeProtocol.mf.OVERLAY_BOX_MODE = [ UINT16, "enableOverlayBox" ];

  NativeProtocol.mf.TEXTURE_2D = [ UINT16, "textureId",
                                   UINT16, "textureType",
                                   UINT16, "mipmapLevel",
                                   UINT16, "format",
                                   UINT16, "dataType",
                                   UINT16, "width",
                                   UINT16, "height",
                                   UINT16, "pixelDataSize",
                                   ARRAY,  "pixelData" ];
  NativeProtocol.mf.SUBTEXTURE_2D = [ UINT16, "textureId",
                                      UINT16, "flags",
                                      UINT16, "xOffset",
                                      UINT16, "yOffset",
                                      UINT16, "width",
                                      UINT16, "height",
                                      ARRAY, "pixelData" ];
  NativeProtocol.mf.DELETE_TEXTURE = [ UINT16, "textureId" ];

  NativeProtocol.mf.CREATE_VERTEX_BUFFER = [ UINT16, "vertexBufferId",
                                             UINT16, "vertexType" ];
  NativeProtocol.mf.FILL_VERTEX_BUFFER = [ UINT16, "vertexBufferId",
                                           UINT16, "vertexSubBuffer",
                                           UINT16, "flags",
                                           UINT32, "subBufferSize",
                                           UINT32, "subBufferOffset",
                                           UINT16, "numVertices",
                                           ARRAY,  "vertices" ];
  NativeProtocol.mf.DELETE_VERTEX_BUFFER = [ UINT16, "vertexBufferId" ];

  NativeProtocol.mf.CREATE_INDEX_BUFFER = [ UINT16, "indexBufferId" ];
  NativeProtocol.mf.FILL_INDEX_BUFFER = [ UINT16, "indexBufferId",
                                          UINT16, "indexSubBuffer",
                                          UINT16, "flags",
                                          UINT32, "subBufferSize",
                                          UINT32, "subBufferOffset",
                                          UINT32, "numIndices",
                                          ARRAY,  "indices" ];
  NativeProtocol.mf.DELETE_INDEX_BUFFER = [ UINT16, "indexBufferId" ];

  NativeProtocol.mf.DEFINE_SEQUENCE = [ UINT16, "sequenceId",
                                        UINT16, "numActions",
                                        ARRAY,  "actions" ];
  NativeProtocol.mf.DELETE_SEQUENCE = [ UINT16, "sequenceId" ];
  NativeProtocol.mf.DEFINE_SEQUENCE_ORDER = [ UINT16, "numSequences",
                                              ARRAY, "sequenceIds" ];
  NativeProtocol.mf.DEFINE_SUB_SEQUENCE = [ UINT16, "sequenceId",
                                            UINT16, "numSequences",
                                            ARRAY, "sequenceIds" ];

  NativeProtocol.mf.SETUP_CAMERA_3D = [ UINT16, "projectionType", // 0: orthographic, 1: perspective
                                        FLOAT32, "zoom",
                                        FLOAT32, "positionX",
                                        FLOAT32, "positionY",
                                        FLOAT32, "positionZ",
                                        FLOAT32, "targetX",
                                        FLOAT32, "targetY",
                                        FLOAT32, "targetZ",
                                        FLOAT32, "upX",
                                        FLOAT32, "upY",
                                        FLOAT32, "upZ",
                                        FLOAT32, "rotCenterX",
                                        FLOAT32, "rotCenterY",
                                        FLOAT32, "rotCenterZ",
                                        FLOAT32, "viewOffsetX",
                                        FLOAT32, "viewOffsetY",
                                        FLOAT32, "scaleX",
                                        FLOAT32, "scaleY",
                                        FLOAT32, "scaleZ" ];
  NativeProtocol.mf.SETUP_CAMERA_2D = [ UINT16, "drawMinX",
                                        UINT16, "drawMaxX",
                                        UINT16, "drawMinY",
                                        UINT16, "drawMaxY",
                                        FLOAT32, "xAxisMin",
                                        FLOAT32, "xAxisMax",
                                        FLOAT32, "yAxisMin",
                                        FLOAT32, "yAxisMax" ];

  NativeProtocol.mf.USE_LIGHT = [ UINT16, "useLight" ];

  NativeProtocol.mf.COORDINATE_BOX = [ FLOAT32, "xCenter",
                                       FLOAT32, "yCenter",
                                       FLOAT32, "zCenter",
                                       UINT16,  "vertexBufferId",
                                       UINT16,  "vertexSubBuffer",
                                       UINT16,  "yzOffset",
                                       UINT16,  "yzNumLines",
                                       FLOAT32, "yzDistance",
                                       UINT16,  "zxOffset",
                                       UINT16,  "zxNumLines",
                                       FLOAT32, "zxDistance",
                                       UINT16,  "xyOffset",
                                       UINT16,  "xyNumLines",
                                       FLOAT32, "xyDistance",
                                       UINT32,  "lineColor",
                                       FLOAT32, "labelsX1DistY",
                                       FLOAT32, "labelsX1DistZ",
                                       UINT16,  "labelsX1SeqId",
                                       FLOAT32, "labelsY1DistX",
                                       FLOAT32, "labelsY1DistZ",
                                       UINT16,  "labelsY1SeqId",
                                       FLOAT32, "labelsZ1DistX",
                                       FLOAT32, "labelsZ1DistY",
                                       UINT16,  "labelsZ1SeqId",
                                       FLOAT32, "labelsX2DistY",
                                       FLOAT32, "labelsX2DistZ",
                                       UINT16,  "labelsX2SeqId",
                                       FLOAT32, "labelsY2DistX",
                                       FLOAT32, "labelsY2DistZ",
                                       UINT16,  "labelsY2SeqId",
                                       FLOAT32, "labelsZ2DistX",
                                       FLOAT32, "labelsZ2DistY",
                                       UINT16,  "labelsZ2SeqId" ];

  NativeProtocol.mf.DEFINE_BACKGROUND = [ UINT32, "topColor",
                                          UINT32, "bottomColor" ];

  NativeProtocol.mf.CONNECTION_INITIATED = [ ];

  NativeProtocol.sd = {}; // Structure definitions used by message format definitions
  NativeProtocol.sd.ACTION_TYPE = [ UINT16, "actionType",
                                    ARRAY,  "actionStart"];
  NativeProtocol.sd.RENDER_INDEXED_ELEMENTS = [ UINT16, "padding",
                                                UINT16, "vertexBufferId",
                                                UINT16, "vertexSubBuffer",
                                                UINT16, "indexBufferId",
                                                UINT16, "indexSubBuffer",
                                                UINT16, "textureId",
                                                UINT16, "elementType",
                                                UINT16, "numIntervals",
                                                ARRAY,  "intervals" ];
  NativeProtocol.sd.RENDER_VERTEXED_ELEMENTS = [ UINT16, "padding",
                                        UINT16, "vertexBufferId",
                                        UINT16, "vertexSubBuffer",
                                        UINT16, "textureId",
                                        UINT16, "elementType",
                                        UINT16, "numIntervals",
                                        ARRAY,  "intervals" ];
  NativeProtocol.sd.RENDER_LABEL_3D = [ UINT16,  "padding",
                                        FLOAT32, "anchorX",
                                        FLOAT32, "anchorY",
                                        FLOAT32, "anchorZ",
                                        UINT16,  "vertexBufferId",
                                        UINT16,  "vertexSubBuffer",
                                        UINT16,  "indexBufferId",
                                        UINT16,  "indexSubBuffer",
                                        UINT16,  "textureId",
                                        UINT16,  "elementType",
                                        UINT16,  "numIntervals",
                                        ARRAY,   "intervals" ];
  NativeProtocol.sd.INTERVAL = [ UINT32, "indexBufferOffset",
                                 UINT32, "numElements",
                                 ARRAY, "endMark" ];
  NativeProtocol.sd.SET_RENDERING_LAYER = [ UINT16, "layerType",
                                            ARRAY, "endMark" ];
  NativeProtocol.sd.SET_ATTRIBUTE_POINT_SIZE = [ FLOAT32, "pointSize",
                                                 ARRAY, "endMark"];
  NativeProtocol.sd.SET_ATTRIBUTE_LINE_WIDTH = [ FLOAT32, "lineWidth",
                                                 ARRAY, "endMark"];
  NativeProtocol.sd.SET_ATTRIBUTE_POLYGON_LAYER = [ UINT16, "polygonLayer",
                                                    ARRAY, "endMark"];
  NativeProtocol.sd.SET_ATTRIBUTE_CULL_FACE = [ UINT16, "cullface",
                                                 ARRAY, "endMark"];
    
  NativeProtocol.sd.SET_ATTRIBUTE_COLOR = [ UINT32 , "color",
                                            ARRAY, "endMark"];
  NativeProtocol.sd.SET_ATTRIBUTE_TRANSPARENCY = [ UINT16 , "transparency",
                                                   ARRAY, "endMark"];

  NativeProtocol.sd.SET_ATTRIBUTE_MATERIAL = [ UINT32 , "materialType",
                                               UINT32 , "ambientColor",
                                               UINT32 , "diffuseColor",
                                               UINT32 , "specularColor",
                                               ARRAY, "endMark"];
    
  NativeProtocol.sd.SET_STATE_MODEL_TRANSFORMATION = [ FLOAT32 , "m11",
                                                 FLOAT32 , "m12",
                                                 FLOAT32 , "m13",
                                                 FLOAT32 , "m14",
                                                 FLOAT32 , "m21",
                                                 FLOAT32 , "m22",
                                                 FLOAT32 , "m23",
                                                 FLOAT32 , "m24",
                                                 FLOAT32 , "m31",
                                                 FLOAT32 , "m32",
                                                 FLOAT32 , "m33",
                                                 FLOAT32 , "m34",
                                                 FLOAT32 , "m41",
                                                 FLOAT32 , "m42",
                                                 FLOAT32 , "m43",
                                                 FLOAT32 , "m44",
                                                 ARRAY, "endMark"];

  NativeProtocol.sd.SET_STATE_DEPTH_TEST = [ UINT16 , "depthTest",
                                       ARRAY, "endMark"];
  
  NativeProtocol.sd.SET_STATE_TEXTURE_TRANSFORMATION = [ UINT16, "textureUnit",
                                                         FLOAT32 , "m11",
                                                         FLOAT32 , "m12",
                                                         FLOAT32 , "m13",
                                                         FLOAT32 , "m14",
                                                         FLOAT32 , "m21",
                                                         FLOAT32 , "m22",
                                                         FLOAT32 , "m23",
                                                         FLOAT32 , "m24",
                                                         FLOAT32 , "m31",
                                                         FLOAT32 , "m32",
                                                         FLOAT32 , "m33",
                                                         FLOAT32 , "m34",
                                                         FLOAT32 , "m41",
                                                         FLOAT32 , "m42",
                                                         FLOAT32 , "m43",
                                                         FLOAT32 , "m44",
                                                         ARRAY, "endMark" ];

  // Texture types
  NativeProtocol.tt = {};
  NativeProtocol.tt.GL_TEXTURE_2D = 0x0DE1;

  // Texture formats
  NativeProtocol.tfmt = {};
  NativeProtocol.tfmt.GL_ALPHA           = 0x1906;
  NativeProtocol.tfmt.GL_RGB             = 0x1907;
  NativeProtocol.tfmt.GL_RGBA            = 0x1908;
  NativeProtocol.tfmt.GL_LUMINANCE       = 0x1909;
  NativeProtocol.tfmt.GL_LUMINANCE_ALPHA = 0x190A;

  // Texture data types
  NativeProtocol.tdt = {};
  NativeProtocol.tdt.GL_UNSIGNED_BYTE = 0x1401;
  NativeProtocol.tdt.GL_UNSIGNED_INT8888 = 0x8035;

  // Texture communication flags
  NativeProtocol.tflg = {};
  NativeProtocol.tflg.FIRST_MSG = 0x0001;
  NativeProtocol.tflg.LAST_MSG  = 0x0002;

  // Vertex buffer types
  NativeProtocol.vt = {};
  NativeProtocol.vt.COORD_NORMAL_TEX         = 0x0001;
  NativeProtocol.vt.COORD                    = 0x0002;
  NativeProtocol.vt.COORD_COLOR              = 0x0003;
  NativeProtocol.vt.COORD2D_TEX              = 0x0004;
  NativeProtocol.vt.COORD_TEX                = 0x0005;
  NativeProtocol.vt.COORD_NORMAL             = 0x0006;
  NativeProtocol.vt.COORD_NORMAL_COLOR       = 0x0007;
  NativeProtocol.vt.COORD_NORMAL_TEX_COLOR   = 0x0008;
  NativeProtocol.vt.COORD_NORMAL_TEX_COMPACT = 0x0101;

  // Vertex buffer communication flags
  NativeProtocol.vbflg = {};
  NativeProtocol.vbflg.FIRST_MSG_SUB_BUF = 0x0001;
  NativeProtocol.vbflg.LAST_MSG_SUB_BUF  = 0x0002;
  NativeProtocol.vbflg.FIRST_MSG_BUF     = 0x0004;
  NativeProtocol.vbflg.LAST_MSG_BUF      = 0x0008;

  // Index buffer communication flags
  NativeProtocol.ibflg = {};
  NativeProtocol.ibflg.FIRST_MSG_SUB_BUF = 0x0001;
  NativeProtocol.ibflg.LAST_MSG_SUB_BUF  = 0x0002;
  NativeProtocol.ibflg.FIRST_MSG_BUF     = 0x0004;
  NativeProtocol.ibflg.LAST_MSG_BUF      = 0x0008;

  // Rendering action types
  NativeProtocol.ra = {};
  NativeProtocol.ra.RENDER_INDEXED_ELEMENTS     = 0x0001;
  NativeProtocol.ra.RENDER_VERTEXED_ELEMENTS    = 0x0002;
  NativeProtocol.ra.RENDER_LABEL_3D             = 0x0081;
  NativeProtocol.ra.RENDER_LABEL_3D_2           = 0x0082;
  NativeProtocol.ra.SET_RENDERING_LAYER         = 0x0101;
  NativeProtocol.ra.SET_ATTRIBUTE_POINT_SIZE    = 0x0201;
  NativeProtocol.ra.SET_ATTRIBUTE_LINE_WIDTH    = 0x0202;
  NativeProtocol.ra.SET_ATTRIBUTE_POLYGON_LAYER = 0x0203;
  NativeProtocol.ra.SET_ATTRIBUTE_COLOR         = 0x0204;
  NativeProtocol.ra.SET_ATTRIBUTE_TRANSPARENCY  = 0x0205;
  NativeProtocol.ra.SET_ATTRIBUTE_CULL_FACE     = 0x0206;
  NativeProtocol.ra.SET_ATTRIBUTE_MATERIAL      = 0x0207;  
  NativeProtocol.ra.SET_STATE_MODEL_TRANSFORMATION = 0x0301;
  NativeProtocol.ra.SET_STATE_DEPTH_TEST = 0x0302;
  NativeProtocol.ra.SET_STATE_TEXTURE_TRANSFORMATION = 0x0402;

  // Render mateiral types
  NativeProtocol.rmt = {};
  NativeProtocol.rmt.NONE      = 0x0001;
  NativeProtocol.rmt.FLATCOLOR = 0x0002;
  NativeProtocol.rmt.PHONG     = 0x0003;
  NativeProtocol.rmt.OTHER     = 0x0004;
    
  // Rendering renderer types
  NativeProtocol.rt = {};
  NativeProtocol.rt.WEBRENDERER_GL_3D    = 0x0001;
  NativeProtocol.rt.WEBRENDERER_IMAGE_3D = 0x0002;
  NativeProtocol.rt.WEBRENDERER_IMAGE_2D = 0x0004;

  // Rendering layer types
  NativeProtocol.rl = {};
  NativeProtocol.rl.SETUP      = 0x0001;
  NativeProtocol.rl.BACKGROUND = 0x0002;
  NativeProtocol.rl.SCENE      = 0x0003;
  NativeProtocol.rl.TRIAD      = 0x0004;
  NativeProtocol.rl.DECORATION = 0x0005;

  // Face Culling types
  NativeProtocol.fct = {};
  NativeProtocol.fct.NONE    = 0x0000;
  NativeProtocol.fct.FRONT   = 0x0001;
  NativeProtocol.fct.BACK    = 0x0002;
  
  // Element types
  NativeProtocol.et = {};
  NativeProtocol.et.GL_POINTS         = 0x0000;
  NativeProtocol.et.GL_LINES          = 0x0001;
  NativeProtocol.et.GL_LINE_LOOP      = 0x0002;
  NativeProtocol.et.GL_LINE_STRIP     = 0x0003;
  NativeProtocol.et.GL_TRIANGLES      = 0x0004;
  NativeProtocol.et.GL_TRIANGLE_STRIP = 0x0005;
  NativeProtocol.et.GL_TRIANGLE_FAN   = 0x0006;

  /** Unpack a byte buffer into "obj" according to "structDef". */
  function unpackFromBuffer(arrayBuf, startIndex, structDef, obj) {
    var nFields = structDef.length / 2;
    var buf = new Uint8Array(arrayBuf);
    var bi = startIndex; // Buffer index
    for (var i = 0; i < nFields; i++) {
      var name = structDef[2*i+1];
      switch (structDef[2*i]) {
      case UINT32:
        obj[name] = buf[bi] + (buf[bi+1]<<8) + (buf[bi+2]<<16) + (buf[bi+3]<<24);
        bi += 4;
        break;
      case UINT16:
        obj[name] = buf[bi] + (buf[bi+1]<<8);
        bi += 2;
        break;
      case UINT8:
        obj[name] = buf[bi];
        bi += 1;
        break;
      case FLOAT32:
        obj[name] = (new DataView(arrayBuf, bi, 4)).getFloat32(0, true);
        bi += 4;
        break;
      case ARRAY:
        if (i + 1 < nFields)
          throw "ARRAY field must be at end of structure";
        obj[name] = bi;
        break;
      default:
        throw "Invalid structure definition:" + structDef[2*i];
      }
    }
    return obj;
  }

  /** Unpack a native message from an ArrayBuffer. */
  NativeProtocol.unpackMessage = function(buffer) {
    var message = {};
    unpackFromBuffer(buffer, 0, NativeProtocol.mf.HEADER, message);
    switch (message.msgType) {
    case NativeProtocol.mt.IMAGE_DATA:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.IMAGE_DATA, message);
      message.data = new Uint8Array(buffer, message.data);
      break;
    case NativeProtocol.mt.SCENE_FINISHED:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.SCENE_FINISHED, message);
      break;
    case NativeProtocol.mt.SET_RENDERER:
        unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.SET_RENDERER, message);
        break;
    case NativeProtocol.mt.OVERLAY_BOX_MODE:
        unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.OVERLAY_BOX_MODE, message);
        break;
    case NativeProtocol.mt.TEXTURE_2D:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.TEXTURE_2D, message);
      message.pixelData = new Uint8Array(buffer, message.pixelData, message.pixelDataSize);
      break;
    case NativeProtocol.mt.SUBTEXTURE_2D:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.SUBTEXTURE_2D, message);
      message.pixelData = new Uint8Array(buffer, message.pixelData);
      break;
    case NativeProtocol.mt.DELETE_TEXTURE:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DELETE_TEXTURE, message);
      break;
    case NativeProtocol.mt.CREATE_VERTEX_BUFFER:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.CREATE_VERTEX_BUFFER, message);
      break;
    case NativeProtocol.mt.FILL_VERTEX_BUFFER:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.FILL_VERTEX_BUFFER, message);
      var vtxStart = message.vertices;
      message.verticesFlt32 = new Float32Array(buffer, message.vertices);
      message.verticesInt32 = new Uint32Array(buffer, message.vertices);
      break;
    case NativeProtocol.mt.DELETE_VERTEX_BUFFER:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DELETE_VERTEX_BUFFER, message);
      break;
    case NativeProtocol.mt.CREATE_INDEX_BUFFER:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.CREATE_INDEX_BUFFER, message);
      break;
    case NativeProtocol.mt.FILL_INDEX_BUFFER:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.FILL_INDEX_BUFFER, message);
      message.indices = new Uint16Array(buffer, message.indices);
      break;
    case NativeProtocol.mt.DELETE_INDEX_BUFFER:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DELETE_INDEX_BUFFER, message);
      break;
    case NativeProtocol.mt.DEFINE_SEQUENCE:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DEFINE_SEQUENCE, message);
      var byteIndex = message.actions;
      message.actions = [];
      for (var i = 0; i < message.numActions; i++) {
        var action = {};
        unpackFromBuffer(buffer, byteIndex, NativeProtocol.sd.ACTION_TYPE, action);
        switch (action.actionType) {
        case NativeProtocol.ra.RENDER_INDEXED_ELEMENTS:
        case NativeProtocol.ra.RENDER_VERTEXED_ELEMENTS:
        case NativeProtocol.ra.RENDER_LABEL_3D:
        case NativeProtocol.ra.RENDER_LABEL_3D_2:
          if (action.actionType == NativeProtocol.ra.RENDER_INDEXED_ELEMENTS)
            unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.RENDER_INDEXED_ELEMENTS, action);
          else if (action.actionType == NativeProtocol.ra.RENDER_VERTEXED_ELEMENTS)
            unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.RENDER_VERTEXED_ELEMENTS, action);
          else
            unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.RENDER_LABEL_3D, action);
          var ivalStart = action.intervals;
          action.intervals = [];
          for (var j = 0; j < action.numIntervals; j++) {
            var interval = {};
            unpackFromBuffer(buffer, ivalStart, NativeProtocol.sd.INTERVAL, interval);
            ivalStart = interval.endMark;
            delete interval.endMark;
            action.intervals.push(interval);
          }
          delete action.numIntervals;
          byteIndex = ivalStart;
          break;
        case NativeProtocol.ra.SET_RENDERING_LAYER:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_RENDERING_LAYER, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_ATTRIBUTE_POINT_SIZE:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_ATTRIBUTE_POINT_SIZE, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_ATTRIBUTE_LINE_WIDTH:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_ATTRIBUTE_LINE_WIDTH, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_ATTRIBUTE_POLYGON_LAYER:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_ATTRIBUTE_POLYGON_LAYER, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_ATTRIBUTE_CULL_FACE:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_ATTRIBUTE_CULL_FACE, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_ATTRIBUTE_COLOR:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_ATTRIBUTE_COLOR, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_ATTRIBUTE_MATERIAL:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_ATTRIBUTE_MATERIAL, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_ATTRIBUTE_TRANSPARENCY:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_ATTRIBUTE_TRANSPARENCY, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_STATE_MODEL_TRANSFORMATION:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_STATE_MODEL_TRANSFORMATION, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_STATE_DEPTH_TEST:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_STATE_DEPTH_TEST, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        case NativeProtocol.ra.SET_STATE_TEXTURE_TRANSFORMATION:
          unpackFromBuffer(buffer, action.actionStart, NativeProtocol.sd.SET_STATE_TEXTURE_TRANSFORMATION, action);
          byteIndex = action.endMark;
          delete action.endMark;
          break;
        default:
          throw "Invalid action type: " + action.actionType;
        }
        delete action.actionStart;
        message.actions.push(action);
      }
      delete message.numActions;
      break;
    case NativeProtocol.mt.DELETE_SEQUENCE:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DELETE_SEQUENCE, message);
      break;
    case NativeProtocol.mt.DEFINE_SEQUENCE_ORDER:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DEFINE_SEQUENCE_ORDER, message);
      message.sequenceIds = new Uint16Array(buffer, message.sequenceIds, message.numSequences);
      delete message.numSequences;
      break;
    case NativeProtocol.mt.DEFINE_SUB_SEQUENCE:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DEFINE_SUB_SEQUENCE, message);
      message.sequenceIds = new Uint16Array(buffer, message.sequenceIds, message.numSequences);
      delete message.numSequences;
      break;
    case NativeProtocol.mt.SETUP_CAMERA_3D:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.SETUP_CAMERA_3D, message);
      break;
    case NativeProtocol.mt.SETUP_CAMERA_2D:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.SETUP_CAMERA_2D, message);
      break;
    case NativeProtocol.mt.USE_LIGHT:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.USE_LIGHT, message);
      break;
    case NativeProtocol.mt.COORDINATE_BOX:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.COORDINATE_BOX, message);
      var abgr = message.lineColor;
      var r = abgr & 0x000000ff;
      var g = (abgr & 0x0000ff00) >>> 8;
      var b = (abgr & 0x00ff0000) >>> 16;
      message.lineColor = (r << 16) | (g << 8) | b;
      break;
    case NativeProtocol.mt.DEFINE_BACKGROUND:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.DEFINE_BACKGROUND, message);
      break;
    case NativeProtocol.mt.CONNECTION_INITIATED:
      unpackFromBuffer(buffer, message.msgData, NativeProtocol.mf.CONNECTION_INITIATED, message);
      break;
    default:
      throw "Invalid message type: " + message.msgType;
    }
    delete message.msgData;
    NativeProtocol.logToConsole(message);
    return message;
  };

  /** Write message to console log if debugging is enabled. */
  if (COMSOL.Logger.isDebugEnabled()) {
    NativeProtocol.logToConsole = function(message) {
      var txt = "";
      for (var prop in message) {
        var value = message[prop];
        switch (prop) {
        case "msgType":
          value = lookupType(NativeProtocol.mt, value);
          break;
        case "vertexType":
          value = lookupType(NativeProtocol.vt, value);
          break;
        case "flags":
          value = undefined;
          break;
        case "lineColor":
          value = "0x" + value.toString(16);
          break;
        case "textureType":
          value = lookupType(NativeProtocol.tt, value);
          break;
        case "format":
          value = lookupType(NativeProtocol.tfmt, value);
          break;
        case "dataType":
          value = lookupType(NativeProtocol.tdt, value);
          break;
        case "sequenceIds":
          value = arrayToString(value, false, Infinity);
          break;
        case "indices":
          value = arrayToString(value, false, 0);
          break;
        case "pixelData":
          value = arrayToString(value, true, 0);
          break;
        case "data":
          value = arrayToString(value, true, 0);
          break;
        case "verticesFlt32":
          value = arrayToString(value, false, 0);
          break;
        case "verticesInt32":
//        value = arrayToString(value, false, 0);
          value = undefined;
          break;
        case "actions":
          value = actionsToString(value);
          break;
        case "renderer":
          value = lookupType(NativeProtocol.rt, value);
          break;
        default:
          break;
        }
        if (value === undefined)
          continue;

        if (txt)
          txt += " ";
        txt += prop + ":" + value;
      }
      COMSOL.Logger.debug("-> " + txt);

      function actionsToString(actions) {
        var txt = "";
        for (var a in actions) {
          var action = actions[a];
          var actTxt = "";
          for (var ap in action) {
            var actProp = action[ap];
            switch (ap) {
            case "padding":
              actProp = undefined;
              break;
            case "actionType":
              actProp = lookupType(NativeProtocol.ra, actProp);
              break;
            case "layerType":
              actProp = lookupType(NativeProtocol.rl, actProp);
              break;
            case "intervals":
              actProp = intervalsToString(actProp);
              break;
            default:
              break;
            }
            if (actProp === undefined)
              continue;
            if (actTxt)
              actTxt += " ";
            actTxt += ap + ":" + actProp;
          }
          txt += "\n  " + actTxt;
        }
        return txt;
      }

      function intervalsToString(intervals) {
        var txt = "";
        for (var i in intervals) {
          var intv = intervals[i];
          var intvTxt = "";
          for (var ip in intv) {
            var intvProp = intv[ip];
            switch (ip) {
            case "elementType":
              intvProp = lookupType(NativeProtocol.et, intvProp);
              break;
            default:
              break;
            }
            if (intvTxt)
              intvTxt += " ";
            intvTxt += ip + ":" + intvProp;
          }
          txt += "\n    " + intvTxt;
        }
        return txt;
      }

      function arrayToString(arr, hex, maxLen) {
        maxLen = maxLen || 0;
        var txt = "(" + arr.length + ")";
        if (maxLen > 0) {
          txt += "[";
          for (var i = 0; i < arr.length; i++) {
            if (i > 0)
              txt += " ";
            if (i >= maxLen) {
              txt += "...";
              break;
            }
            if (hex)
              txt += "0x" + arr[i].toString(16);
            else
              txt += arr[i];
          }
          txt += "]";
        }
        return txt;
      }

      function lookupType(typeDef, value) {
        for (var t in typeDef)
          if (typeDef[t] == value)
            return t;
        return value;
      }

    };
  } else
    NativeProtocol.logToConsole = function() {};

  return NativeProtocol;
})();

// ------------------------------------------------------------

/** Defines a texture. */
COMSOL.NativeTexture = (function() {
  var NativeProtocol = COMSOL.NativeProtocol;

  function NativeTexture(message) {
    if (message.msgType !== NativeProtocol.mt.TEXTURE_2D)
      throw "Invalid message type";
    if (message.textureType !== NativeProtocol.tt.GL_TEXTURE_2D)
      throw "Invalied texture type";
    if ((message.dataType !== NativeProtocol.tdt.GL_UNSIGNED_BYTE) &&
        (message.dataType !== NativeProtocol.tdt.GL_UNSIGNED_INT8888))
      throw "Invalid texture data type";

    this.textureId = message.textureId;
    this.textureType = message.textureType;
    this.mipmapLevel = message.mipmapLevel;
    this.format = message.format;
    this.dataType = message.dataType;
    this.width = message.width;
    this.height = message.height;

    if ((message.dataType === NativeProtocol.tdt.GL_UNSIGNED_INT8888) &&
        (this.bytesPerPixel() != 4))
      throw "Incompatible texture data type";

    var pds = this.width * this.height * this.bytesPerPixel();
    this.pixelData = new Uint8Array(pds);
    var src = message.pixelData;
    var dst = this.pixelData;
    var len = message.pixelDataSize;
    if (this.dataType === NativeProtocol.tdt.GL_UNSIGNED_INT8888) {
      for (var i = 0; i < len; i++)
        dst[i+3-2*(i%4)] = src[i];
    } else {
      for (var i = 0; i < len; i++)
        dst[i] = src[i];
    }

    this.modified = true;
  };

  NativeTexture.prototype.setSubTexture = function(message) {
    if (message.msgType !== NativeProtocol.mt.SUBTEXTURE_2D)
      throw "Invalid message type";
    var xb = message.xOffset;
    var xe = xb + message.width;
    var yb = message.yOffset;
    var ye = yb + message.height;
    var src = message.pixelData;
    var dst = this.pixelData;
    var dstW = this.width;
    var bpp = this.bytesPerPixel();
    var i = 0;

    if (this.dataType === NativeProtocol.tdt.GL_UNSIGNED_INT8888) {
      for (var y = yb; y < ye; y++)
        for (var x = xb; x < xe; x++)
          for (var b = bpp-1; b >= 0; b--)
            dst[(y*dstW + x)*bpp+b] = src[i++];
    } else {
      for (var y = yb; y < ye; y++)
        for (var x = xb; x < xe; x++)
          for (var b = 0; b < bpp; b++)
            dst[(y*dstW + x)*bpp+b] = src[i++];
    }
    this.modified = true;
  };

  NativeTexture.prototype.bytesPerPixel = function() {
    switch (this.format) {
    case NativeProtocol.tfmt.GL_ALPHA:
      return 1;
    case NativeProtocol.tfmt.GL_RGB:
      return 3;
    case NativeProtocol.tfmt.GL_RGBA:
      return 4;
    case NativeProtocol.tfmt.GL_LUMINANCE:
      return 1;
    case NativeProtocol.tfmt.GL_LUMINANCE_ALPHA:
      return 2;
    default:
      throw "Invalid texture format";
    }
  };

  /** Evaluate the color of a texture in coordinate (u,v). */
  NativeTexture.prototype.evalColor = function(u, v) {
    var x = Math.max(0, Math.min(this.width - 1, Math.floor(u * this.width)));
    var y = Math.max(0, Math.min(this.height - 1, Math.floor(v * this.height)));
    var idx = y * this.width + x;
    switch (this.format) {
    case NativeProtocol.tfmt.GL_ALPHA:
      return new THREE.Color(0, 0, 0);
    case NativeProtocol.tfmt.GL_RGB:
      return new THREE.Color(this.pixelData[idx*3]/255,
                             this.pixelData[idx*3+1]/255,
                             this.pixelData[idx*3+2]/255);
    case NativeProtocol.tfmt.GL_RGBA:
      return new THREE.Color(this.pixelData[idx*4]/255,
                             this.pixelData[idx*4+1]/255,
                             this.pixelData[idx*4+2]/255);
    case NativeProtocol.tfmt.GL_LUMINANCE:
      return new THREE.Color(this.pixelData[idx]/255,
                             this.pixelData[idx]/255,
                             this.pixelData[idx]/255);
    case NativeProtocol.tfmt.GL_LUMINANCE_ALPHA:
      return new THREE.Color(this.pixelData[idx*2]/255,
                             this.pixelData[idx*2]/255,
                             this.pixelData[idx*2]/255);
    default:
      throw "Invalid texture format";
    }
  };

  /** Create a canvas representation of the texture. */
  NativeTexture.prototype.createCanvas = function() {
    var w = this.width;
    var h = this.height;
    var canvas = document.createElement("canvas");
    canvas.width = w;
    canvas.height = h;

    var context = canvas.getContext("2d");
    var imageData = context.getImageData(0, 0, w, h);
    var dw = imageData.width;  // Width in device pixels
    var dh = imageData.height; // Height in device pixels
    var wFactor = w / dw;
    var hFactor = h / dh;
    var bpp = this.bytesPerPixel();
    var data = imageData.data;
    var textureData = this.pixelData;
    var format = this.format;
    for (var dy = 0; dy < dh; dy++) {
      var y = dy * hFactor;
      for (var dx = 0; dx < dw; dx++) {
        var x = dx * wFactor;
        var idx;
        switch (format) {
        case NativeProtocol.tfmt.GL_ALPHA:
          data[(dy*dw+dx)*4+3] = textureData[y*w+x];
          break;
        case NativeProtocol.tfmt.GL_RGB:
          data[(dy*dw+dx)*4  ] = textureData[(y*w+x)*3];
          data[(dy*dw+dx)*4+1] = textureData[(y*w+x)*3+1];
          data[(dy*dw+dx)*4+2] = textureData[(y*w+x)*3+2];
          data[(dy*dw+dx)*4+3] = 255;
          break;
        case NativeProtocol.tfmt.GL_RGBA:
          data[(dy*dw+dx)*4  ] = textureData[(y*w+x)*4];
          data[(dy*dw+dx)*4+1] = textureData[(y*w+x)*4+1];
          data[(dy*dw+dx)*4+2] = textureData[(y*w+x)*4+2];
          data[(dy*dw+dx)*4+3] = textureData[(y*w+x)*4+3];
          break;
        case NativeProtocol.tfmt.GL_LUMINANCE:
          data[(dy*dw+dx)*4  ] = textureData[(y*w+x)];
          data[(dy*dw+dx)*4+1] = textureData[(y*w+x)];
          data[(dy*dw+dx)*4+2] = textureData[(y*w+x)];
          data[(dy*dw+dx)*4+3] = 255;
          break;
        case NativeProtocol.tfmt.GL_LUMINANCE_ALPHA:
          data[(dy*dw+dx)*4  ] = textureData[(y*w+x)*2];
          data[(dy*dw+dx)*4+1] = textureData[(y*w+x)*2];
          data[(dy*dw+dx)*4+2] = textureData[(y*w+x)*2];
          data[(dy*dw+dx)*4+3] = textureData[(y*w+x)*2+1];
          break;
        default:
          throw "Invalid texture format";
        }
      }
    }
    context.putImageData(imageData, 0, 0);
    return canvas;
  }

  return NativeTexture;
})();


COMSOL.NativeVertexSubBuffer = (function() {
  var NativeProtocol = COMSOL.NativeProtocol;

  function NativeVertexSubBuffer(vertexType, numVertices) {
    this.numVertices = numVertices;
    this.vertexType = vertexType;

    switch (vertexType) {
    case NativeProtocol.vt.COORD_NORMAL_TEX:
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      this.uv = new Float32Array(numVertices * 2);
      break;
    case NativeProtocol.vt.COORD:
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      break;
    case NativeProtocol.vt.COORD_COLOR:
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      this.color = new Float32Array(numVertices * 3);
      break;
    case NativeProtocol.vt.COORD2D_TEX:
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      this.uv = new Float32Array(numVertices * 2);
      break;
    case NativeProtocol.vt.COORD_TEX:
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      this.uv = new Float32Array(numVertices * 2);
      break;
    case NativeProtocol.vt.COORD_NORMAL:
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      break;
    case NativeProtocol.vt.COORD_NORMAL_COLOR:
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      this.color = new Float32Array(numVertices * 3);
      break;
    case NativeProtocol.vt.COORD_NORMAL_TEX_COLOR: // Color ignored
      this.position = new Float32Array(numVertices * 3);
      this.normal = new Float32Array(numVertices * 3);
      this.uv = new Float32Array(numVertices * 2);
      break;
    default:
      throw "Vertex type not supported, type: " + vertexType;
    }

    this.modified = true;
  }

  NativeVertexSubBuffer.prototype.fillBuffer = function(vtxOffset, nVtxToFill,
                                                        bufFlt32, bufInt32) {
    var i = 0;
    switch (this.vertexType) {
    case NativeProtocol.vt.COORD_NORMAL_TEX:
      var p = this.position;
      var n = this.normal;
      var uv = this.uv;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = bufFlt32[i++];
        n[v*3+0] = bufFlt32[i++];
        n[v*3+1] = bufFlt32[i++];
        n[v*3+2] = bufFlt32[i++];
        uv[2*v+0] = bufFlt32[i++];
        uv[2*v+1] = bufFlt32[i++];
      }
      break;
    case NativeProtocol.vt.COORD:
      var p = this.position;
      var n = this.normal;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = bufFlt32[i++];
        n[v*3+0] = 0;
        n[v*3+1] = 0;
        n[v*3+2] = 1;
      }
      break;
    case NativeProtocol.vt.COORD_COLOR:
      var p = this.position;
      var n = this.normal;
      var c = this.color;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = bufFlt32[i++];
        n[v*3+0] = 0;
        n[v*3+1] = 0;
        n[v*3+2] = 1;
        var abgr = bufInt32[i++];
        c[v*3+0] = (abgr & 0x000000ff) / 255;
        c[v*3+1] = ((abgr & 0x0000ff00) >>> 8) / 255;
        c[v*3+2] = ((abgr & 0x00ff0000) >>> 16) / 255;
      }
      break;
    case NativeProtocol.vt.COORD2D_TEX:
      var p = this.position;
      var uv = this.uv;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = 0;
        n[v*3+0] = 0;
        n[v*3+1] = 0;
        n[v*3+2] = 1;
        uv[v*2+0] = bufFlt32[i++];
        uv[v*2+1] = bufFlt32[i++];
      }
      break;
    case NativeProtocol.vt.COORD_TEX:
      var p = this.position;
      var n = this.normal;
      var uv = this.uv;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = bufFlt32[i++];
        n[v*3+0] = 0;
        n[v*3+1] = 0;
        n[v*3+2] = 1;
        uv[2*v+0] = bufFlt32[i++];
        uv[2*v+1] = bufFlt32[i++];
      }
      break;
    case NativeProtocol.vt.COORD_NORMAL:
      var p = this.position;
      var n = this.normal;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = bufFlt32[i++];
        n[v*3+0] = bufFlt32[i++];
        n[v*3+1] = bufFlt32[i++];
        n[v*3+2] = bufFlt32[i++];
      }
      break;
    case NativeProtocol.vt.COORD_NORMAL_COLOR:
      var p = this.position;
      var n = this.normal;
      var c = this.color;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = bufFlt32[i++];
        n[v*3+0] = bufFlt32[i++];
        n[v*3+1] = bufFlt32[i++];
        n[v*3+2] = bufFlt32[i++];
        var abgr = bufInt32[i++];
        c[v*3+0] = (abgr & 0x000000ff) / 255;
        c[v*3+1] = ((abgr & 0x0000ff00) >>> 8) / 255;
        c[v*3+2] = ((abgr & 0x00ff0000) >>> 16) / 255;
        // Note! Vertex alpha not supported by three.js
      }
      break;
    case NativeProtocol.vt.COORD_NORMAL_TEX_COLOR:
      var p = this.position;
      var n = this.normal;
      var uv = this.uv;
      for (var v = vtxOffset; v < vtxOffset + nVtxToFill; v++) {
        p[v*3+0] = bufFlt32[i++];
        p[v*3+1] = bufFlt32[i++];
        p[v*3+2] = bufFlt32[i++];
        n[v*3+0] = bufFlt32[i++];
        n[v*3+1] = bufFlt32[i++];
        n[v*3+2] = bufFlt32[i++];
        var abgr = bufInt32[i++]; // Ignored
        uv[2*v+0] = bufFlt32[i++];
        uv[2*v+1] = bufFlt32[i++];
      }
      break;
    default:
      throw "Vertex type not supported, type: " + vertexType;
    }

    this.modified = true;
  };

  return NativeVertexSubBuffer;
})();

/** Defines a vertex buffer which contains a number of sub buffers. */
COMSOL.NativeVertexBuffer = (function() {
  var NativeProtocol = COMSOL.NativeProtocol;

  function NativeVertexBuffer(message) {
    if (message.msgType !== NativeProtocol.mt.CREATE_VERTEX_BUFFER)
      throw "Invalid message type";
    this.vertexBufferId = message.vertexBufferId;
    this.vertexType = message.vertexType;
    this.subBuffers = []; // Elements have type NativeVertexSubBuffer
  }

  NativeVertexBuffer.prototype.fillBuffer = function(message) {
    if (message.msgType !== NativeProtocol.mt.FILL_VERTEX_BUFFER)
      throw "Invalid message type";
    var bi = message.vertexSubBuffer;
    var subSize = message.subBufferSize;
    if (!this.subBuffers[bi])
      this.subBuffers[bi] = new COMSOL.NativeVertexSubBuffer(this.vertexType, subSize);
    else if (this.subBuffers[bi].numVertices !== subSize)
      throw "Inconsistent sub buffer size";
    this.subBuffers[bi].fillBuffer(message.subBufferOffset, message.numVertices,
                                   message.verticesFlt32, message.verticesInt32);
  };

  return NativeVertexBuffer;
})();


/** Defines an index buffer, which contains a number of sub buffers. */
COMSOL.NativeIndexBuffer = (function() {
  var NativeProtocol = COMSOL.NativeProtocol;

  function NativeIndexBuffer(message) {
    if (message.msgType !== NativeProtocol.mt.CREATE_INDEX_BUFFER)
      throw "Invalid message type";
    this.indexBufferId = message.indexBufferId;
    this.subBuffers = []; // Elements have type Uint16Array
    this.subBuffersModified = [];
  }

  NativeIndexBuffer.prototype.fillBuffer = function(message) {
    if (message.msgType !== NativeProtocol.mt.FILL_INDEX_BUFFER)
      throw "Invalid message type";
    var bi = message.indexSubBuffer;
    var subSize = message.subBufferSize;
    if (!this.subBuffers[bi])
      this.subBuffers[bi] = new Uint16Array(subSize);
    else if (this.subBuffers[bi].length !== subSize)
      throw "Inconsistent sub buffer size";
    var src = message.indices;
    var dst = this.subBuffers[bi];
    var offs = message.subBufferOffset;
    var len = message.numIndices;
    for (var i = 0; i < len; i++)
      dst[offs+i] = src[i];

    this.subBuffersModified[bi] = true;
  };

  return NativeIndexBuffer;
})();


/** Defines a rendering action. Rendering actions can render elements
 * and can set state for future element rendering operations. */
COMSOL.NativeRenderAction = (function() {
  function NativeRenderAction(messageAction) {
    for (var p in messageAction)
      if (messageAction.hasOwnProperty(p))
        this[p] = messageAction[p];
  }

  return NativeRenderAction;
})();

/** A sequence of render actions. */
COMSOL.NativeRenderSequence = (function() {
  var nextObjectId = 1;

  function NativeRenderSequence() {
    this.objectId = nextObjectId++;
    this.actions = []; // Elements have type NativeRenderAction
  }

  return NativeRenderSequence;
})();

/** A list of sequence IDs. */
COMSOL.NativeSequenceList = (function() {
  function NativeSequenceList(message) {
    this.sequenceIds = message.sequenceIds;
  }

  return NativeSequenceList;
})();

// ------------------------------------------------------------

/** Defines a rendering action that renders elements. */
COMSOL.NativeRenderElements = (function() {
  var NativeProtocol = COMSOL.NativeProtocol;

  function NativeRenderElements(renderAction, objectId, attributes, states) {
    switch (renderAction.actionType) {
      case NativeProtocol.ra.RENDER_INDEXED_ELEMENTS:
      case NativeProtocol.ra.RENDER_VERTEXED_ELEMENTS:
        break;
      case NativeProtocol.ra.RENDER_LABEL_3D:
      case NativeProtocol.ra.RENDER_LABEL_3D_2:
        this.anchorX = renderAction.anchorX;
        this.anchorY = renderAction.anchorY;
        this.anchorZ = renderAction.anchorZ;
        break;
      default:
        throw "Wrong render action type";
    }
    this.vertexBufferId = renderAction.vertexBufferId;
    this.vertexSubBuffer = renderAction.vertexSubBuffer;
    if (renderAction.actionType != NativeProtocol.ra.RENDER_VERTEXED_ELEMENTS) {
      this.indexBufferId = renderAction.indexBufferId;
      this.indexSubBuffer = renderAction.indexSubBuffer;
    }
    this.textureId = renderAction.textureId;
    this.intervals = renderAction.intervals;
    this.elementType = renderAction.elementType;
    this.objectId = objectId;
    if (attributes !== undefined && attributes instanceof COMSOL.RenderAttribute) {
      this.lineWidth = attributes.lineWidth;
      this.pointSize = attributes.pointSize;
      this.polygonLayer = attributes.polygonLayer;
      this.cullface = attributes.cullface;  
      this.color = attributes.color;
      this.transparency = attributes.transparency;
      this.material = attributes.material;
    }
    if (states !== undefined && states instanceof COMSOL.RenderState) {
      this.modelTransformation = states.modelTransformation;
      this.depthTest = states.depthTest == 0 ? false : true;
      this.textureTransformation = states.textureTransformation;
    }
    this.renderTexture = undefined;
  }

  /** Return object opacity value, between 0 and 1. */
  NativeRenderElements.prototype.getOpacity = function() {
    return this.transparency ? 0.5 : 1.0;
  };
  
  NativeRenderElements.prototype.build_renderTexture = function(threeTexture, repeatx, repeaty, offsetx, offsety) {
    COMSOL.RenderTexture.prototype = threeTexture;
    this.renderTexture = new COMSOL.RenderTexture(repeatx, repeaty, offsetx, offsety);
  };

  return NativeRenderElements;
})();


/** Handle the rendering attribute. */
COMSOL.RenderAttribute = (function() {
  function RenderAttribute() {
    this.lineWidth = 1;
    this.pointSize = 1;
    this.polygonLayer = 1;
    this.cullface = COMSOL.NativeProtocol.fct.NONE;
    this.color = new THREE.Color(0, 0, 0);
    this.transparency = 0;
    this.material = undefined;
  }

  RenderAttribute.prototype.assign = function(obj) {
    this.lineWidth = obj.lineWidth;
    this.pointSize = obj.pointSize;
    this.polygonLayer = obj.polygonLayer;
    this.cullface = obj.cullface;
    this.color = obj.color;
    this.transparency = obj.transparency;
    this.material = obj.material;
  };

  return RenderAttribute;
})();

/** Handle the rendering state. */
COMSOL.RenderState = (function() {
  function RenderState() {
    this.modelTransformation = undefined;
    this.depthTest = undefined;
    this.textureTransformation = undefined;
  }

  RenderState.prototype.assign = function(obj) {
    this.modelTransformation = obj.modelTransformation;
    this.depthTest = obj.depthTest;
    this.textureTransformation = obj.textureTransformation;
  };

  return RenderState;
})();

COMSOL.RenderTexture = (function() {
  function RenderTexture(repeatx, repeaty, offsetx, offsety) {
    this.repeat = new THREE.Vector2(repeatx, repeaty);
    this.offset = new THREE.Vector2(offsetx, offsety);
  }
  return RenderTexture;
})();

/** Handle display of the background gradient. */
COMSOL.Background = (function() {
  function Background() {
    this.topColor = 0;
    this.bottomColor = 0;
    this.mesh = null;
  }

  Background.prototype.update = function(sceneLayer, topColor, bottomColor) {
    if (topColor != null)
      this.topColor = topColor;
    if (bottomColor != null)
      this.bottomColor = bottomColor;

    var tb = ((this.topColor    & 0x00ff0000) >>> 16) / 255;
    var tg = ((this.topColor    & 0x0000ff00) >>> 8 ) / 255;
    var tr = ((this.topColor    & 0x000000ff)       ) / 255;
    var bb = ((this.bottomColor & 0x00ff0000) >>> 16) / 255;
    var bg = ((this.bottomColor & 0x0000ff00) >>> 8 ) / 255;
    var br = ((this.bottomColor & 0x000000ff)       ) / 255;

    var w = sceneLayer.nativeScene.width;
    var h = sceneLayer.nativeScene.height;

    var pos = new Float32Array(12);
    pos[0] = 0; pos[1]  = 0; pos[2]  = 0;
    pos[3] = w; pos[4]  = 0; pos[5]  = 0;
    pos[6] = 0; pos[7]  = h; pos[8]  = 0;
    pos[9] = w; pos[10] = h; pos[11] = 0;

    var color = new Float32Array(12);
    color[0] = br; color[1]  = bg; color[2]  = bb;
    color[3] = br; color[4]  = bg; color[5]  = bb;
    color[6] = tr; color[7]  = tg; color[8]  = tb;
    color[9] = tr; color[10] = tg; color[11] = tb;

    if (!this.mesh) {
      var normal = new Float32Array(12);
      normal[0] = 0; normal[1]  = 0; normal[2]  = 1;
      normal[3] = 0; normal[4]  = 0; normal[5]  = 1;
      normal[6] = 0; normal[7]  = 0; normal[8]  = 1;
      normal[9] = 0; normal[10] = 0; normal[11] = 1;

      var idxBuf = new Uint16Array(6);
      idxBuf[0] = 0; idxBuf[1] = 1; idxBuf[2] = 2;
      idxBuf[3] = 1; idxBuf[4] = 3; idxBuf[5] = 2;

      var geom = new THREE.BufferGeometry();
      geom.addAttribute('position', new THREE.BufferAttribute(pos, 3));
      geom.addAttribute('normal', new THREE.BufferAttribute(normal, 3));
      geom.addAttribute('color', new THREE.BufferAttribute(color, 3));
      geom.setIndex(new THREE.BufferAttribute(idxBuf, 1));
      geom.addGroup(0,6);

      var mtrl = new THREE.MeshBasicMaterial({vertexColors: THREE.VertexColors});
      mtrl.transparent = true;
      mtrl.depthTest = false;
      mtrl.depthWrite = false;
      mtrl.side = THREE.DoubleSide;

      var mesh = new THREE.Mesh(geom, mtrl);
      mesh.name = "background";
      this.mesh = mesh;
      sceneLayer.world.add(mesh);
    } else {
      var geom = this.mesh.geometry;
      geom.getAttribute('position').array = pos;
      geom.getAttribute('position').needsUpdate = true;
      geom.getAttribute('color').array = color;
      geom.getAttribute('color').needsUpdate = true;
    }
  }

  return Background;
})();


/** Handle display of the coordinate box. */
COMSOL.CoordinateBox = (function() {
  function LabelData(direction) {
    this.axisDirection = direction; // 0,1,2 corresponding to X,Y,Z
    this.axisLabels = [];           // THREE.Object3D objects holding axis label objects
    this.distance1 = 0; // (half) dimension of bounding box
    this.distance2 = 0; // (half) dimension of bounding box
    this.distance10 = 0; // (half) dimension of bounding box + plane offset
    this.distance20 = 0; // (half) dimension of bounding box + plane offset
    // padding:
    // * the amount to distance itself with from the edge of the bounding box
    // * this value is set in `update` and used in `move`
    this.padding = 0;
  };

  /** Remove all label objects from scene. */
  LabelData.prototype.clear = function(scene) {
    for (var j = 0; j < this.axisLabels.length; j++)
      scene.remove(this.axisLabels[j]);
    this.axisLabels.length = 0;
  };

  /**
   * Moves label based on the camera position.
   * 
   * @param crdBox   The bounding box.
   * @param edges    Array specifying which edges should have the labels.
   * @param isNears  Array specifying whether the above array of edges are the near edges.
   */
  LabelData.prototype.move = function(crdBox, edges, isNears) {
    for (var i = 0; i < this.axisLabels.length; i++) {
      var label = this.axisLabels[i];
      switch (this.axisDirection) {
      case 0:
        label.offsetPos.x = 0;
        label.offsetPos.y = crdBox.yCenter + (edges[0] ? 1 : -1) * (isNears[0] ? this.distance1 : this.distance10);
        label.offsetPos.z = crdBox.zCenter + (edges[1] ? 1 : -1) * (isNears[1] ? this.distance2 : this.distance20);
        
        label.normalPos.x = 0;
        label.normalPos.y = (edges[0] ? 1 : -1) * this.padding;
        label.normalPos.z = (edges[1] ? 1 : -1) * this.padding;
        break;
      case 1:
        label.offsetPos.x = crdBox.xCenter + (edges[1] ? 1 : -1) * (isNears[1] ? this.distance2 : this.distance20);
        label.offsetPos.y = 0;
        label.offsetPos.z = crdBox.zCenter + (edges[0] ? 1 : -1) * (isNears[0] ? this.distance1 : this.distance10);
        
        label.normalPos.x = (edges[1] ? 1 : -1) * this.padding;
        label.normalPos.y = 0;
        label.normalPos.z = (edges[0] ? 1 : -1) * this.padding;
        break;
      case 2:
        label.offsetPos.x = crdBox.xCenter + (edges[0] ? 1 : -1) * (isNears[0] ? this.distance1 : this.distance10);
        label.offsetPos.y = crdBox.yCenter + (edges[1] ? 1 : -1) * (isNears[1] ? this.distance2 : this.distance20);
        label.offsetPos.z = 0;
        
        label.normalPos.x = (edges[0] ? 1 : -1) * this.padding;
        label.normalPos.y = (edges[1] ? 1 : -1) * this.padding;
        label.normalPos.z = 0;
        break;
      }
    }
  };

  /** Constructor */
  function CoordinateBox() {
    // Bounding box center
    this.xCenter = 0;
    this.yCenter = 0;
    this.zCenter = 0;

    // THREE.Object3D objects holding coordinate plane objects
    this.xyPlane = null;
    this.yzPlane = null;
    this.zxPlane = null;

    // Distance from bounding box center to coordinate planes
    this.xyDistance = 0;
    this.yzDistance = 0;
    this.zxDistance = 0;

    this.labelsX1 = new LabelData(0);
    this.labelsY1 = new LabelData(1);
    this.labelsZ1 = new LabelData(2);
    this.labelsX2 = new LabelData(0);
    this.labelsY2 = new LabelData(1);
    this.labelsZ2 = new LabelData(2);
  }

  /** Return all label objects. */
  CoordinateBox.prototype.getAllLabels = function() {
    var labels = [];
    function append(labelData) {
      Array.prototype.push.apply(labels, labelData.axisLabels);
    }
    append(this.labelsX1);
    append(this.labelsY1);
    append(this.labelsZ1);
    append(this.labelsX2);
    append(this.labelsY2);
    append(this.labelsZ2);
    return labels;
  };

  /** Add/remove THREE objects used for coordinate box display. */
  CoordinateBox.prototype.update = function(sceneLayer, crdBox) {
    var scene = sceneLayer.scene;
    var vertexBuffers = sceneLayer.nativeScene.vertexBuffers;

    // Delete old data
    var planeNames = [ "xy", "yz", "zx" ];
    for (var i = 0; i < 3; i++) {
      scene.remove(this[planeNames[i]+"Plane"]);
      this[planeNames[i]+"Plane"] = null;
    }
    this.labelsX1.clear(scene);
    this.labelsY1.clear(scene);
    this.labelsZ1.clear(scene);
    this.labelsX2.clear(scene);
    this.labelsY2.clear(scene);
    this.labelsZ2.clear(scene);

    if (!crdBox || !crdBox.vertexBufferId)
      return;
    var vBuf = vertexBuffers[crdBox.vertexBufferId];
    if (!vBuf) {
      COMSOL.Logger.error("Coordinate box, missing vertex buffer: " + crdBox.vertexBufferId);
      return;
    }
    var vtxBuf = vBuf.subBuffers[crdBox.vertexSubBuffer];
    if (!vtxBuf)
      return;

    this.xCenter = crdBox.xCenter;
    this.yCenter = crdBox.yCenter;
    this.zCenter = crdBox.zCenter;
    this.xyDistance = crdBox.xyDistance;
    this.yzDistance = crdBox.yzDistance;
    this.zxDistance = crdBox.zxDistance;

    // Create geometries for coordinate planes
    for (var i = 0; i < 3; i++) {
      var planeObj = new THREE.Object3D();
      planeObj.name = "coord_plane_" + i;
      scene.add(planeObj);
      this[planeNames[i]+"Plane"] = planeObj;

      var geom = new THREE.BufferGeometry();
      geom.addAttribute('position', new THREE.BufferAttribute(vtxBuf.position, 3));
      var count = crdBox[planeNames[i]+"NumLines"] * 2;
      var offs = crdBox[planeNames[i]+"Offset"];
      var idxBuf = new Uint16Array(count);
      for (var j = 0; j < count; j++)
        idxBuf[j] = offs + j;
      geom.setIndex(new THREE.BufferAttribute(idxBuf, 1));
      geom.addGroup(0, count);
      COMSOL.NativeSceneLayer.computeBufferGeomBBox(geom);

      var mtrl = new THREE.LineBasicMaterial({color: crdBox.lineColor});
      var mesh = new THREE.LineSegments(geom, mtrl);
      mesh.name = "coord_plane_" + i;

      planeObj.add(mesh);
    }

    // Create geometries for axis labels
    var indexBuffers = sceneLayer.nativeScene.indexBuffers;
    var threeTextures = sceneLayer.nativeScene.threeTextures;
    function updateLabelData(labelData, seqId, dist1, dist2, dist10, dist20) {
      labelData.distance1 = dist1;
      labelData.distance2 = dist2;
      labelData.distance10 = dist10;
      labelData.distance20 = dist20;
      var sqPadding = 0; // the greatest (squared) padding of any label along this axis
      if (seqId) {
        var axisLabels = labelData.axisLabels;
        var sequences = sceneLayer.nativeScene.getSequences(seqId);
        var RENDER_LABEL_3D = COMSOL.NativeProtocol.ra.RENDER_LABEL_3D;
        for (var seqIdx = 0; seqIdx < sequences.length; seqIdx++) {
          var seq = sequences[seqIdx];
          for (var j = 0; j < seq.actions.length; j++) {
            var act = seq.actions[j];
            if (act.actionType == RENDER_LABEL_3D) {
              var re = new COMSOL.NativeRenderElements(act, -1, 1, 1);
              var intv = re.intervals[0];
              var count = intv.numElements * 3; // Triangles
              var vtxBuf = vertexBuffers[re.vertexBufferId].subBuffers[re.vertexSubBuffer];
              var idxBuf = indexBuffers[re.indexBufferId].subBuffers[re.indexSubBuffer];
              var texture = threeTextures[re.textureId];
              if (!texture) {
                COMSOL.Logger.error("Axis label, missing texture: " + re.textureId);
                continue;
              }
              
              // compute (squared) padding of this particular label, possibly overwriting the greatest value
              if (vtxBuf.position.length / 3 != 4)
                COMSOL.Logger.error("Some label is not a square!");
              // we compute width, height and diagonal since we do not know (or care!) which one is which; but mostly because we do not know
              var d1x = vtxBuf.position[1 * 3 + 0] - vtxBuf.position[0 * 3 + 0];
              var d1y = vtxBuf.position[1 * 3 + 1] - vtxBuf.position[0 * 3 + 1];
              var d1z = vtxBuf.position[1 * 3 + 2] - vtxBuf.position[0 * 3 + 2];
              var d1 = d1x * d1x + d1y * d1y + d1z * d1z;
              var d2x = vtxBuf.position[2 * 3 + 0] - vtxBuf.position[0 * 3 + 0];
              var d2y = vtxBuf.position[2 * 3 + 1] - vtxBuf.position[0 * 3 + 1];
              var d2z = vtxBuf.position[2 * 3 + 2] - vtxBuf.position[0 * 3 + 2];
              var d2 = d2x * d2x + d2y * d2y + d2z * d2z;
              var d3x = vtxBuf.position[3 * 3 + 0] - vtxBuf.position[0 * 3 + 0];
              var d3y = vtxBuf.position[3 * 3 + 1] - vtxBuf.position[0 * 3 + 1];
              var d3z = vtxBuf.position[3 * 3 + 2] - vtxBuf.position[0 * 3 + 2];
              var d3 = d3x * d3x + d3y * d3y + d3z * d3z;
              if (d1 > sqPadding) sqPadding = d1;
              if (d2 > sqPadding) sqPadding = d2;
              if (d3 > sqPadding) sqPadding = d3;

              // setup label
              var geom = new THREE.BufferGeometry();
              geom.addAttribute('position', new THREE.BufferAttribute(vtxBuf.position, 3));
              geom.addAttribute('normal', new THREE.BufferAttribute(vtxBuf.normal, 3));
              geom.addAttribute('uv', new THREE.BufferAttribute(vtxBuf.uv, 2));
              geom.setIndex(new THREE.BufferAttribute(idxBuf, 1));
              geom.addGroup(intv.indexBufferOffset, count);
              var mtrl = new THREE.MeshBasicMaterial({vertexColors: THREE.NoColors});
              mtrl.transparent = true;
              mtrl.alphaTest = 1/256;
              mtrl.map = texture;
              mtrl.side = THREE.DoubleSide;
              var mesh = new THREE.Mesh(geom, mtrl);
              mesh.name = "coord_label_mesh " + seqIdx + "," + j;
              var label = new THREE.Object3D();
              label.name = "coord_label " + seqIdx + "," + j;
              label.anchorPos = new THREE.Vector3(re.anchorX, re.anchorY, re.anchorZ);
              label.offsetPos = new THREE.Vector3();
              label.normalPos = new THREE.Vector3();
              label.add(mesh);
              axisLabels.push(label);
              scene.add(label);
            }
          }
        }
      }
      // set the padding of this axis
      labelData.padding = Math.sqrt(sqPadding) / 2;
    }
    updateLabelData(this.labelsX1, crdBox.labelsX1SeqId, crdBox.labelsX1DistY, crdBox.labelsX1DistZ, crdBox.zxDistance, crdBox.xyDistance);
    updateLabelData(this.labelsY1, crdBox.labelsY1SeqId, crdBox.labelsY1DistZ, crdBox.labelsY1DistX, crdBox.xyDistance, crdBox.yzDistance);
    updateLabelData(this.labelsZ1, crdBox.labelsZ1SeqId, crdBox.labelsZ1DistX, crdBox.labelsZ1DistY, crdBox.yzDistance, crdBox.zxDistance);
    updateLabelData(this.labelsX2, crdBox.labelsX2SeqId, crdBox.labelsX2DistY, crdBox.labelsX2DistZ, crdBox.zxDistance, crdBox.xyDistance);
    updateLabelData(this.labelsY2, crdBox.labelsY2SeqId, crdBox.labelsY2DistZ, crdBox.labelsY2DistX, crdBox.xyDistance, crdBox.yzDistance);
    updateLabelData(this.labelsZ2, crdBox.labelsZ2SeqId, crdBox.labelsZ2DistX, crdBox.labelsZ2DistY, crdBox.yzDistance, crdBox.zxDistance);
    // the exponents need more padding, depending on the padding of the ticks
    this.labelsX2.padding += 2 * this.labelsX1.padding;
    this.labelsY2.padding += 2 * this.labelsY1.padding;
    this.labelsZ2.padding += 2 * this.labelsZ1.padding;
  };

  /** Move coordinate box geometries based on camera position. */
  CoordinateBox.prototype.move = function(camera) {
    if (camera.isMode2D())
      return;
    // find which corner is closest to the camera
    var cameraInverse = new THREE.Matrix4().getInverse(camera.matrix);
    var xDist = this.yzDistance;
    var yDist = this.zxDistance;
    var zDist = this.xyDistance;
    var distClosest, xiClosest, yiClosest, ziClosest;
    if (camera.isPerspectiveProjection()) {
      for (var xi = 0; xi < 2; xi++) {
        var xCrd = this.xCenter + xDist * (xi ? 1 : -1);
        for (var yi = 0; yi < 2; yi++) {
          var yCrd = this.yCenter + yDist * (yi ? 1 : -1);
          for (var zi = 0; zi < 2; zi++) {
            var zCrd = this.zCenter + zDist * (zi ? 1 : -1);
            var v = new THREE.Vector3(xCrd, yCrd, zCrd);
            v.applyMatrix4(cameraInverse);
            var dist = v.length();
            var idx = xi*4+yi*2+zi;
            if ((idx == 0) || (distClosest > dist)) {
              distClosest = dist;
              xiClosest = xi;
              yiClosest = yi;
              ziClosest = zi;
            }
          }
        }
      }
    } else {
      xiClosest = (new THREE.Vector4(1,0,0,0)).applyMatrix4(cameraInverse).z < 0 ? 0 : 1;
      yiClosest = (new THREE.Vector4(0,1,0,0)).applyMatrix4(cameraInverse).z < 0 ? 0 : 1;
      ziClosest = (new THREE.Vector4(0,0,1,0)).applyMatrix4(cameraInverse).z < 0 ? 0 : 1;
    }
    
    // figure out which edges should have labels
    var minx = this.xCenter - xDist;
    var miny = this.yCenter - yDist;
    var minz = this.zCenter - zDist;
    var maxx = this.xCenter + xDist;
    var maxy = this.yCenter + yDist;
    var maxz = this.zCenter + zDist;
    var corners = [new THREE.Vector3(minx, miny, minz), new THREE.Vector3(minx, miny, maxz),
                   new THREE.Vector3(minx, maxy, minz), new THREE.Vector3(minx, maxy, maxz),
                   new THREE.Vector3(maxx, miny, minz), new THREE.Vector3(maxx, miny, maxz),
                   new THREE.Vector3(maxx, maxy, minz), new THREE.Vector3(maxx, maxy, maxz)];
    for (var i = 0; i < 8; i++) {
      corners[i].applyProjection(cameraInverse);
      corners[i].applyProjection(camera.projectionMatrix);
    }
    var edges = [new THREE.Vector3().subVectors(corners[1], corners[0]),
                 new THREE.Vector3().subVectors(corners[2], corners[0]),
                 new THREE.Vector3().subVectors(corners[4], corners[0]),
                 new THREE.Vector3().subVectors(corners[3], corners[7]),
                 new THREE.Vector3().subVectors(corners[5], corners[7]),
                 new THREE.Vector3().subVectors(corners[6], corners[7])];
    var face_normals = [new THREE.Vector3().crossVectors(edges[1], edges[2]).normalize(),
                        new THREE.Vector3().crossVectors(edges[2], edges[0]).normalize(),
                        new THREE.Vector3().crossVectors(edges[0], edges[1]).normalize(),
                        new THREE.Vector3().crossVectors(edges[4], edges[5]).normalize(),
                        new THREE.Vector3().crossVectors(edges[5], edges[3]).normalize(),
                        new THREE.Vector3().crossVectors(edges[3], edges[4]).normalize()];
    var edge_normals = [new THREE.Vector3().addVectors(face_normals[0], face_normals[1]), // Xs...
                        new THREE.Vector3().addVectors(face_normals[0], face_normals[4]),
                        new THREE.Vector3().addVectors(face_normals[5], face_normals[1]),
                        new THREE.Vector3().addVectors(face_normals[5], face_normals[4]),
                        new THREE.Vector3().addVectors(face_normals[0], face_normals[2]), // Ys...
                        new THREE.Vector3().addVectors(face_normals[0], face_normals[3]),
                        new THREE.Vector3().addVectors(face_normals[5], face_normals[2]),
                        new THREE.Vector3().addVectors(face_normals[5], face_normals[3]),
                        new THREE.Vector3().addVectors(face_normals[1], face_normals[2]), // Zs...
                        new THREE.Vector3().addVectors(face_normals[1], face_normals[3]),
                        new THREE.Vector3().addVectors(face_normals[4], face_normals[2]),
                        new THREE.Vector3().addVectors(face_normals[4], face_normals[3])];
    var nearx = xiClosest;
    var neary = yiClosest;
    var nearz = ziClosest;
    var farx = 1 - xiClosest;
    var fary = 1 - yiClosest;
    var farz = 1 - ziClosest;
    // in projection space, we always look down the (negaitve) z-axis, i.e. (0.f, 0.f, -1.f)
    // therefore, dotting with the edge normals implies looking at the negative z-component
    var yz = edge_normals[0 + fary + 2 * nearz].z - edge_normals[0 + neary + 2 * farz].z > 0.0001 ? [fary, nearz] : [neary, farz];
    var zx = edge_normals[4 + farx + 2 * nearz].z - edge_normals[4 + nearx + 2 * farz].z > 0.0001 ? [nearz, farx] : [farz, nearx];
    var xy = edge_normals[8 + farx + 2 * neary].z - edge_normals[8 + nearx + 2 * fary].z > 0.0001 ? [farx, neary] : [nearx, fary];

    if (this.xyPlane)
      this.xyPlane.position.set(0, 0, this.zCenter + zDist*(ziClosest?-1:1));
    if (this.yzPlane)
      this.yzPlane.position.set(this.xCenter + xDist*(xiClosest?-1:1), 0, 0);
    if (this.zxPlane)
      this.zxPlane.position.set(0, this.yCenter + yDist*(yiClosest?-1:1), 0);
    this.labelsX1.move(this, yz, [yz[0] == yiClosest, yz[1] == ziClosest]);
    this.labelsY1.move(this, zx, [zx[0] == ziClosest, zx[1] == xiClosest]);
    this.labelsZ1.move(this, xy, [xy[0] == xiClosest, xy[1] == yiClosest]);
    this.labelsX2.move(this, yz, [yz[0] == yiClosest, yz[1] == ziClosest]);
    this.labelsY2.move(this, zx, [zx[0] == ziClosest, zx[1] == xiClosest]);
    this.labelsZ2.move(this, xy, [xy[0] == xiClosest, xy[1] == yiClosest]);
  };

  return CoordinateBox;
})();

/** Handles image objects which are used when WebGL is not used. */
COMSOL.NativeImage = (function() {
  function NativeImage(scene) {
    this.scene = scene;
    this.image = document.createElement("img");
    this.empty = true;
    this.currentURL = null;
  }

  /** Set image for a layer. */
  NativeImage.prototype.setImage = function(data) {
    if (this.currentURL) {
      window.URL.revokeObjectURL(this.currentURL);
      this.currentURL = null;
    }
    if (data && data.length > 0) {
      var blob = new Blob([data], {type: "image/png"});
      this.currentURL = window.URL.createObjectURL(blob);
      this.image.src = this.currentURL;
      this.empty = false;
      var nativeScene = this.scene;
      this.image.onload = function() {
        nativeScene.renderScene();
      };
    } else {
      this.empty = true;
    }
  };

  /** Draw the image if it has been loaded. */
  NativeImage.prototype.draw = function(context, w, h) {
    if (!this.empty && this.image.complete)
      context.drawImage(this.image, 0, 0, w, h);
  };

  return NativeImage;
})();

/** Defines a scene layer. */
COMSOL.NativeSceneLayer = (function() {
  var NativeProtocol = COMSOL.NativeProtocol;

  function NativeSceneLayer(nativeScene, layerType, nativeImage) {
    this.nativeScene = nativeScene;
    this.layerType = layerType;
    this.renderElementsArray = []; // Elements have type NativeRenderElements

    this.meshMap = {};  // Map from NativeRenderElements.objectId to THREE.Mesh
    this.labelMap = {}; // Map from NativeRenderElements.objectId to THREE.Object3D
                        // used for implementing label transforms

    this.scene = new THREE.Scene();
    this.world = new THREE.Object3D();
    this.world.name = "world, layer:" + layerType;
    this.scene.add(this.world);
    this.camera = null;
    this.useDepthBuffer = true;
    this.lights = [];
    this.nativeImage = nativeImage;

    var w = this.nativeScene.width;
    var h = this.nativeScene.height;

    switch (layerType) {
    case NativeProtocol.rl.BACKGROUND:
      this.background = new COMSOL.Background();
      // Fall through
    case NativeProtocol.rl.DECORATION:
      this.camera = new THREE.OrthographicCamera(0, w, h, 0, 1, 2000);
      this.camera.up.set(0,1,0);
      this.camera.position.set(0, 0, 1000);
      this.camera.lookAt(new THREE.Vector3(0, 0, 0));
      var light = new THREE.AmbientLight(0);
      light.name = "light";
      this.lights.push(light);
      this.scene.add(light);
      this.useSceneLights(false);
      this.useDepthBuffer = false;
      break;
    case NativeProtocol.rl.SCENE:
      if (this.nativeScene.mouseCtrl) {
        this.camera = this.nativeScene.mouseCtrl.getCamera();
      } else {
        this.camera = new COMSOL.ComsolCamera(45*Math.PI/180, w / h, 0.1, 1000);
        this.camera.up.set(0,0,1);
        this.camera.position.set(-8, -8, 8);
        this.camera.lookAt(new THREE.Vector3(0, 0, 0));
      }
      var light0 = new THREE.AmbientLight(0);
      light0.name = "light0";
      this.lights.push(light0);
      this.scene.add(light0);
      var light1 = new THREE.DirectionalLight(0xffffff, 0.8);
      light1.name = "light1";
      light1.cameraPosition = new THREE.Vector3(-4, -3.7, 5).normalize();
      this.lights.push(light1);
      this.scene.add(light1);
      var light2 = new THREE.DirectionalLight(0xffffff, 0.4);
      light2.name = "light2";
      light2.cameraPosition = new THREE.Vector3(4, 2, 1).normalize();
      this.lights.push(light2);
      this.scene.add(light2);
      var light3 = new THREE.DirectionalLight(0xffffff, 0.55);
      light3.cameraPosition = new THREE.Vector3(-0.5, 5, -4).normalize();
      light3.name = "light3";
      this.lights.push(light3);
      this.scene.add(light3);
      this.useSceneLights(false);
      this.coordinateBox = new COMSOL.CoordinateBox();
      break;
    case NativeProtocol.rl.TRIAD:
      var dx = 40, dy = 40, k = 30;
      this.triadX0 = dx;
      this.triadY0 = dy;
      this.triadSize = k;
      this.camera = new THREE.OrthographicCamera(-dx/k, (-dx+w)/k, (-dy+h)/k, -dy/k,
                                                 -100, 100);
      var light = new THREE.AmbientLight(0xffffff);
      light.name = "light";
      this.lights.push(light);
      this.scene.add(light);
      break;
    case NativeProtocol.rl.SETUP: // Setup layer is not a real layer
    default:
      throw "Unsupported layer type: " + layerType;
    }
  }

  /** Called when window is resized. */
  NativeSceneLayer.prototype.onResize = function(w, h) {
    switch (this.layerType) {
    case NativeProtocol.rl.BACKGROUND:
    case NativeProtocol.rl.DECORATION:
      this.camera.right = w;
      this.camera.top = h;
      this.camera.updateProjectionMatrix();
      if (this.background)
        this.background.update(this, null, null);
      break;
    case NativeProtocol.rl.SCENE:
      this.camera.aspect = w / h;
      this.camera.updateProjectionMatrix();
      break;
    case NativeProtocol.rl.TRIAD:
      var dx = this.triadX0;
      var dy = this.triadY0;
      var k = this.triadSize;
      this.camera = new THREE.OrthographicCamera(-dx/k, (-dx+w)/k, (-dy+h)/k, -dy/k,
                                                 -100, 100);
      break;
    }
  };

  /** Update meshes, meshMap, labelMap and scene to match renderElementsArray. */
  NativeSceneLayer.prototype.updateScene = function() {
    var toKeep = {};
    for (var i = 0; i < this.renderElementsArray.length; i++)
      toKeep[this.renderElementsArray[i].objectId] = true;
    var newMeshMap = {};
    var newLabelMap = {};
    for (var objId in this.meshMap) {
      if (toKeep[objId]) {
        newMeshMap[objId] = this.meshMap[objId];
        if (objId in this.labelMap)
          newLabelMap[objId] = this.labelMap[objId];
      }
      if (objId in this.labelMap)
        this.scene.remove(this.labelMap[objId]);
      else
        this.world.remove(this.meshMap[objId]);
    }
    this.meshMap = newMeshMap;
    this.labelMap = newLabelMap;

    var textures = this.nativeScene.textures;
    var threeTextures = this.nativeScene.threeTextures;
    var vertexBuffers = this.nativeScene.vertexBuffers;
    var indexBuffers = this.nativeScene.indexBuffers;

    // Add new and update modified meshes
    for (var i = 0; i < this.renderElementsArray.length; i++) {
      var re = this.renderElementsArray[i];
      var isLabel = "anchorX" in re;
      var objId = re.objectId;
      var vtxBuf = vertexBuffers[re.vertexBufferId].subBuffers[re.vertexSubBuffer];
      var isUseIndexBuffer = "indexBufferId" in re;
      var idxBuf = isUseIndexBuffer? indexBuffers[re.indexBufferId].subBuffers[re.indexSubBuffer] : null;
      var useTexture = !!(re.textureId != 0 && vtxBuf.uv);
      var useVtxColor = !!vtxBuf.color;
      
      if (useTexture) {
        var repeatx, repeaty, offsetx, offsety;
        if (re.textureTransformation !== undefined) {
          repeatx = re.textureTransformation.elements[0];
          repeaty = re.textureTransformation.elements[5];
          offsetx = re.textureTransformation.elements[12];
          offsety = re.textureTransformation.elements[13];
        } else {
          repeatx = 1;
          repeaty = 1;
          offsetx = 0;
          offsety = 0;
        }
        re.build_renderTexture(threeTextures[re.textureId], repeatx, repeaty, offsetx, offsety);
      }

      if (!this.meshMap[objId]) {
        var geom;
        if (re.elementType != NativeProtocol.et.GL_POINTS) {
          geom = new THREE.BufferGeometry();
          if (isUseIndexBuffer) {
            geom.addAttribute('position', new THREE.BufferAttribute(vtxBuf.position, 3));
            var attrNormal = new THREE.BufferAttribute(vtxBuf.normal, 3);
            attrNormal.needsUpdate = vtxBuf.normal !== undefined && vtxBuf.normal.length != 0;
            geom.addAttribute('normal', attrNormal);
              
            if (useTexture)
              geom.addAttribute('uv', new THREE.BufferAttribute(vtxBuf.uv, 2));
            else if (useVtxColor)
              geom.addAttribute('color', new THREE.BufferAttribute(vtxBuf.color, 3));
            geom.setIndex(new THREE.BufferAttribute(idxBuf, 1));

            // Now loop all the intervals to set mutiple offsets.
            for (var idxInterval = 0; idxInterval < re.intervals.length; idxInterval++) {
              var intv = re.intervals[idxInterval];
              var countPts = this.getVerticesNumberOfElements(re.elementType, intv.numElements);
              geom.addGroup(intv.indexBufferOffset, countPts);
            }
          }
          else {
            // Extract intervals (i.e. segments) into the 'position'
            // Loop all intvals
            var pos = this.extractSubArray( { itemSize: 3, array: vtxBuf.position },
                                            re.intervals, re.elementType);
            geom.addAttribute('position', this.toBufferAttribute(pos));              
            var normal = this.extractSubArray( { itemSize: 3, array: vtxBuf.normal },
                                               re.intervals, re.elementType);
           geom.addAttribute('normal', this.toBufferAttribute(normal, vtxBuf.normal !== undefined && vtxBuf.normal.length != 0));
            if (useTexture) 
                geom.addAttribute('uv', this.toBufferAttribute(this.extractSubArray( { itemSize: 2, array: vtxBuf.uv }, re.intervals, re.elementType)));
            else if (useVtxColor && vtxBuf.color !== undefined && vtxBuf.color.length != 0)
                geom.addAttribute('color', this.toBufferAttribute(this.extractSubArray( { itemSize: 3, array: vtxBuf.color }, re.intervals, re.elementType)));
          }
          NativeSceneLayer.computeBufferGeomBBox(geom);
        }

        var mtrl, mesh;
        switch (re.elementType) {
        case NativeProtocol.et.GL_POINTS:
          mesh = this.makeParticleMesh(vtxBuf, idxBuf, re, textures[re.textureId]);
          break;
        case NativeProtocol.et.GL_LINES:
        case NativeProtocol.et.GL_LINE_STRIP:
          if (useTexture)
            mtrl = new THREE.LineBasicMaterial({linewidth: re.lineWidth,
                                                vertexColors: false});
          else if (useVtxColor)
            mtrl = new THREE.LineBasicMaterial({linewidth: re.lineWidth,
                                                vertexColors: true});
          else
            mtrl = new THREE.LineBasicMaterial({linewidth: re.lineWidth,
                                                color: re.color});
          if (useTexture)
            mtrl.map = re.renderTexture;
          mtrl.transparent = true;
          mtrl.opacity = this.layerType == NativeProtocol.rl.SCENE ? re.getOpacity() : 1.0;
          mtrl.depthTest = this.useDepthBuffer;
          mtrl.depthWrite = this.useDepthBuffer && mtrl.opacity >= 1;
          if (re.elementType == NativeProtocol.et.GL_LINE_STRIP)                
              mesh = new THREE.Line(geom, mtrl);
          else
              mesh = new THREE.LineSegments(geom, mtrl);
          break;
        case NativeProtocol.et.GL_TRIANGLES:
          var colorParam;
          if (useTexture)
            colorParam = {vertexColors: THREE.NoColors};
          else if (useVtxColor)
            colorParam = {vertexColors: THREE.VertexColors};
          else
            colorParam = {color: re.color};
          if (isLabel) {
            mtrl = new THREE.MeshBasicMaterial(colorParam);
            if (this.layerType == NativeProtocol.rl.SCENE ||
                this.layerType == NativeProtocol.rl.TRIAD)
              mtrl.alphaTest = 1/256;
          } else {
              if (this.layerType == NativeProtocol.rl.SCENE) {
                  mtrl = new THREE.MeshPhongMaterial(colorParam);
                  if (typeof re.material !== 'undefined') {
                      mtrl.color = re.material.diffuseColor;
                      mtrl.specular = re.material.specularColor;
                  }
              } else {
                  if (this.nativeScene.useWebGL)
                      mtrl = new THREE.MeshLambertMaterial(colorParam);
                  else
                      mtrl = new THREE.MeshBasicMaterial(colorParam);
              }
          }

          if (useTexture)
            mtrl.map = re.renderTexture;
          mtrl.transparent = true;
          mtrl.opacity = this.layerType == NativeProtocol.rl.SCENE ? re.getOpacity() : 1.0;
          mtrl.depthTest = this.useDepthBuffer;
          mtrl.depthWrite = this.useDepthBuffer && mtrl.opacity >= 1;
          mtrl.side = re.cullface == NativeProtocol.fct.FRONT ? THREE.BackSide : (re.cullface == NativeProtocol.fct.BACK ? THREE.FrontSide : THREE.DoubleSide);
          mtrl.polygonOffset = true;
          mtrl.polygonOffsetFactor = 1;
          mtrl.polygonOffsetUnits = 1;
          mesh = new THREE.Mesh(geom, mtrl);
          break;
        default:
          throw "Unsupported element type: " + re.elementType;
        }
        this.meshMap[objId] = mesh;
      } else {
        var mesh = this.meshMap[objId];
        var geom = mesh.geometry;
        var mtrl = mesh.material;

        var idxModified = isUseIndexBuffer && indexBuffers[re.indexBufferId].subBuffersModified[re.indexSubBuffer];
        if (re.elementType == NativeProtocol.et.GL_POINTS) {
          if ((useTexture && textures[re.textureId].modified) || vtxBuf.modified || idxModified) {
            this.scene.remove(mesh);
            mesh = this.makeParticleMesh(vtxBuf, idxBuf, re, textures[re.textureId]);
            this.scene.add(mesh);
            this.meshMap[objId] = mesh;
          }
        } else {
          if (useTexture && textures[re.textureId].modified) {
            mtrl.map = re.renderTexture;
            mtrl.needsUpdate = true;
          }
          if (vtxBuf.modified) {
            if (isUseIndexBuffer) {
              geom.getAttribute('position').array = vtxBuf.position;
              geom.getAttribute('position').needsUpdate = true;
              if (vtxBuf.normal !== undefined)
                geom.addAttribute('normal', new THREE.BufferAttribute(vtxBuf.normal, 3));
              else
                geom.addAttribute('normal', new THREE.BufferAttribute(new Float32Array(0), 3));
              geom.getAttribute('normal').needsUpdate = vtxBuf.normal !== undefined && vtxBuf.normal.length != 0;
              if (useTexture) {
                geom.getAttribute('uv').array = vtxBuf.uv;
                geom.getAttribute('uv').needsUpdate = true;
              } else if (useVtxColor) {
                geom.getAttribute('color').array = vtxBuf.color;
                geom.getAttribute('color').needsUpdate = true;
              }
            }
            else {
                geom.addAttribute('position', this.toBufferAttribute(this.extractSubArray( { itemSize: 3, array: vtxBuf.position }, re.intervals, re.elementType)));
                geom.getAttribute('position').needsUpdate = true;
              if (vtxBuf.normal !== undefined)
                  geom.addAttribute('normal', this.toBufferAttribute(
                      this.extractSubArray({ itemSize: 3, array: vtxBuf.normal },
                      re.intervals, re.elementType)));
              else
                geom.addAttribute('normal', new THREE.BufferAttribute(new Float32Array(0), 3));
              geom.getAttribute('normal').needsUpdate = vtxBuf.normal !== undefined && vtxBuf.normal.length != 0;
              if (useTexture) {
                  geom.addAttribute('uv', this.toBufferAttribute(
                      this.extractSubArray({ itemSize: 2, array: vtxBuf.uv },
                      re.intervals, re.elementType)));
                geom.getAttribute('uv').needsUpdate = true;
              }
              else if (useVtxColor) {
                  geom.addAttribute('color', this.toBufferAttribute(
                      this.extractSubArray({ itemSize: 3, array: vtxBuf.color },
                      re.intervals, re.elementType)));
                geom.getAttribute('color').needsUpdate = true;
              }
            }
          }
          if (idxModified) {
            geom.getIndex().array = idxBuf;
            geom.getIndex().needsUpdate = true;
            // Now loop all the intervals to set mutiple offsets.
            geom.clearGroups();
            for (var idxInterval = 0; idxInterval < re.intervals.length; idxInterval++) {
              var intv = re.intervals[idxInterval];
              geom.addGroup(intv.indexBufferOffset,
                            this.getVerticesNumberOfElements(re.elementType, intv.numElements));
            }
          }
          if (vtxBuf.modified || idxModified)
            NativeSceneLayer.computeBufferGeomBBox(geom);
        }
      }
      // Assign render state
      if (re.modelTransformation !== undefined) {
        mesh.matrix = re.modelTransformation;
        mesh.matrixAutoUpdate = false;
      }
      if (re.depthTest !== undefined) {
        mesh.material.depthTest = re.depthTest;
      }

      mesh.name = objId;
      if (isLabel) {
        var label = new THREE.Object3D();
        label.name = "label " + objId;
        label.anchorPos = new THREE.Vector3(re.anchorX, re.anchorY, re.anchorZ);
        label.add(mesh);
        this.scene.add(label);
        this.labelMap[objId] = label;
      } else {
        this.world.add(mesh);
      }
    }
  };

  /** Util method to calculate the number of vertices from given
   * elementype and number of elements. */
  NativeSceneLayer.prototype.getVerticesNumberOfElements = function(elementType, numElements) {
    var count = 0;
    switch (elementType) {
    case NativeProtocol.et.GL_POINTS:
      count = numElements;
      break;
    case NativeProtocol.et.GL_LINES:
      count = numElements * 2;
      break;
    case NativeProtocol.et.GL_LINE_STRIP:
      count = numElements + 1;
      break;
    case NativeProtocol.et.GL_TRIANGLES:
      count = numElements * 3;
      break;
    default:
      throw "Unsupported element type: " + elementType;
    }
    return count;
  };

  NativeSceneLayer.prototype.toBufferAttribute = function(bufferArray, needsUpdate) {
      var attribute = new THREE.BufferAttribute(bufferArray.array, bufferArray.itemSize);
    if (needsUpdate !== undefined)
      attribute.needsUpdate = needsUpdate;
    return attribute
  };
    
  /** Util method to extract subarrys from given buffer. */
  NativeSceneLayer.prototype.extractSubArray = function(buffer, intervals, elementType) {
    // For single interval if startindex and number elment are mached, do not extract.
    if (intervals.length == 0)
      return buffer;
    var itemsize = buffer.itemSize;
    if (intervals.length == 1 && intervals[0].indexBufferOffset == 0 &&
        (this.getVerticesNumberOfElements(elementType, intervals[0].numElements)*itemsize ==
          buffer.array.length)) {
      return buffer;
    }
    // Extract sub array for mutiple interval or single longer interval.
    var totalcount = 0;
    for (var i = 0; i < intervals.length; i++) {
      var itv = intervals[i];
      var numVertices = this.getVerticesNumberOfElements(elementType, itv.numElements);
      totalcount += numVertices;
    }

    var newbufferarray = new Float32Array(itemsize*totalcount);
    var totaloffset = 0;
    for (var i = 0; i < intervals.length; i++) {
      var itv = intervals[i];
      var numVertices = this.getVerticesNumberOfElements(elementType, itv.numElements);
      var segment = buffer.array.subarray(itv.indexBufferOffset*itemsize,
          (itv.indexBufferOffset + numVertices)*itemsize);
      newbufferarray.set(segment, totaloffset);
      totaloffset += segment.length;
    }
    var newbuffer = { itemSize:itemsize, array:newbufferarray };
    return newbuffer;
  };

  /** THREE.ParticleSystem does not support indexed geometry.
   * Create a geometry containing only the required vertices. */
  NativeSceneLayer.prototype.makeParticleMesh = function(vtxBuf, idxBuf, re, texture) {
    var geom = new THREE.Geometry();
    for (var i = 0; i < re.intervals.length; i++) {
      var intv = re.intervals[i];
      for (var p = 0; p < intv.numElements; p++) {
        var idx;
        if (idxBuf !== null)
          idx = idxBuf[intv.indexBufferOffset + p];
        else
          idx = intv.indexBufferOffset + p;
        geom.vertices.push(new THREE.Vector3(vtxBuf.position[idx*3],
            vtxBuf.position[idx*3+1],
            vtxBuf.position[idx*3+2]));
        if (texture) {
          geom.colors.push(texture.evalColor(vtxBuf.uv[idx*2],
              vtxBuf.uv[idx*2+1]));
        } else if (vtxBuf.color) {
          geom.colors.push(new THREE.Color(vtxBuf.color[idx*3],
              vtxBuf.color[idx*3+1],
              vtxBuf.color[idx*3+2]));
        }
      }
    }
    var mtrl;
    if (texture || vtxBuf.color)
      mtrl = new THREE.ParticleSystemMaterial({size: re.pointSize,
                                              sizeAttenuation: false,
                                              vertexColors: THREE.VertexColors});
    else
      mtrl = new THREE.ParticleSystemMaterial({size: re.pointSize,
        sizeAttenuation: false,
        color: re.color});

    mtrl.transparent = true;
    mtrl.opacity = this.layerType == NativeProtocol.rl.SCENE ? re.getOpacity() : 1.0;
    mtrl.depthTest = this.useDepthBuffer;
    mtrl.depthWrite = this.useDepthBuffer && mtrl.opacity >= 1;
    return new THREE.ParticleSystem(geom, mtrl);
  };

  /** Transform label objects so they are oriented towards the camera
   * and use a screen pixel coordinate system. */
  NativeSceneLayer.prototype.transformLabels = function() {
    /**
     * Computes the scale of a label.
     *
     * @param isPerspective  True if using perspective rather than orthogonal view.
     * @param label          The label (we only care for its position).
     * @param mInv           The inverse matrix of the label, only used in pserpective view.
     * @param y              Magic number, only used in perspective view.
     * @param orthoScale     The orthogonal scale, only used in orthogonal view.
     * 
     * @return  The scale.
     */
    function computeScale(isPerspective, label, mInv, y, orthoScale) {
      if (isPerspective) {
        var v = label.position.clone();
        v.applyMatrix4(mInv);
        var s = -v.z * 2 / y / window.innerHeight;
        return s;
      } else {
    	  return orthoScale;
      }
    }

    switch (this.layerType) {
    case NativeProtocol.rl.BACKGROUND:
    case NativeProtocol.rl.DECORATION:
      for (var id in this.labelMap) {
        var label = this.labelMap[id];
        label.position.set(label.anchorPos.x, label.anchorPos.y, label.anchorPos.z);
      }
      break;
    case NativeProtocol.rl.SCENE:
      this.camera.updateMatrix();
      var y = this.camera.projectionMatrix.elements[5];
      var cameraInverse = new THREE.Matrix4().getInverse(this.camera.matrix);
      var sx = this.world.scale.x;
      var sy = this.world.scale.y;
      var sz = this.world.scale.z;
      if (this.camera.isMode2D())
        break;
      var isPerspective = this.camera.isPerspectiveProjection();
      var orthoScale = this.camera.orthoScale / window.innerWidth;
      for (var id in this.labelMap) {
        var label = this.labelMap[id];
        label.position.set(label.anchorPos.x * sx, label.anchorPos.y * sy, label.anchorPos.z * sz);
        var s = computeScale(isPerspective, label, cameraInverse, y, orthoScale);
        label.setRotationFromMatrix(this.camera.matrix);
        label.scale.set(s, s, s);
      }
      // Transform coordinate box labels
      var axisLabels = this.coordinateBox.getAllLabels();
      for (var i = 0; i < axisLabels.length; i++) {
        var label = axisLabels[i];
        // set position to be on the edge
        label.position.set(label.anchorPos.x + label.offsetPos.x,
                           label.anchorPos.y + label.offsetPos.y,
                           label.anchorPos.z + label.offsetPos.z);
        // what scale will it have here?
        var s = computeScale(isPerspective, label, cameraInverse, y, orthoScale);
        // offset position from the edge depending on the scale
        label.position.set(label.position.x + label.normalPos.x * s,
                           label.position.y + label.normalPos.y * s,
                           label.position.z + label.normalPos.z * s);
        // apply transformations
        label.setRotationFromMatrix(this.camera.matrix);
        label.scale.set(s, s, s);
      }
      break;
    case NativeProtocol.rl.TRIAD:
      this.camera.updateMatrix();
      for (var id in this.labelMap) {
        var label = this.labelMap[id];
        label.position.set(label.anchorPos.x, label.anchorPos.y, label.anchorPos.z);
        label.setRotationFromMatrix(this.camera.matrix);
        var s = 1 / this.triadSize;
        label.scale.set(s, s, s);
      }
      break;
    }
  };

  /** Transform lights that are defined in the camera coordinate frame. */
  NativeSceneLayer.prototype.transformLights = function() {
    for (var i = 0; i < this.lights.length; i++) {
      var light = this.lights[i];
      if ((light instanceof THREE.DirectionalLight) && light.cameraPosition) {
        var c = light.cameraPosition;
        var v = (new THREE.Vector4(c.x, c.y, c.z, 0)).applyMatrix4(this.camera.matrix);
        v.normalize();
        light.position.set(v.x, v.y, v.z);
      }
    }
  };

  /** Enable/disable use of directional light sources. */
  NativeSceneLayer.prototype.useSceneLights = function(enable) {
    for (var i = 0; i < this.lights.length; i++) {
      var light = this.lights[i];
      if (light instanceof THREE.AmbientLight) {
        if (enable) {
          var ambient = (this.layerType == NativeProtocol.rl.SCENE) ? 0.3 : 1.0;
          light.color = new THREE.Color(ambient, ambient, ambient);
        } else {
          light.color = new THREE.Color(0xd0d0d0);
        }
      } else {
        light.visible = enable;
      }
    }
    for (var m in this.meshMap) {
      if (this.meshMap.hasOwnProperty(m)) {
        var mtrl = this.meshMap[m].material;
        mtrl.needsUpdate = true; // Update shader
      }
    }
  };

  /** Set world x/y/z scale factors. */
  NativeSceneLayer.prototype.setViewScale = function(viewScale) {
    this.world.scale.set(viewScale.scaleX, viewScale.scaleY, viewScale.scaleZ);
  }

  /** Define coordinate box objects. */
  NativeSceneLayer.prototype.defineCoordinateBox = function(crdBox) {
    this.coordinateBox.update(this, crdBox);
  };

  /** Define background gradient. */
  NativeSceneLayer.prototype.defineBackground = function(bg) {
    if (this.layerType != NativeProtocol.rl.BACKGROUND)
      throw "Internal error";
    var topColor = bg && bg.topColor;
    var bottomColor = bg && bg.bottomColor;
    this.background.update(this, topColor, bottomColor);
  };

  /** Setup the 3D camera view. */
  NativeSceneLayer.prototype.setupCamera3D = function(camDef) {
    if (!(this.camera instanceof COMSOL.ComsolCamera))
      throw "Internal error";
    this.camera.setView3D(camDef);
    this.setViewScale(camDef);
  };

  /** Setup the 2D camera view. */
  NativeSceneLayer.prototype.setupCamera2D = function(camDef) {
    if (!(this.camera instanceof COMSOL.ComsolCamera))
      throw "Internal error";
    this.camera.setView2D(camDef);
  };

  /** Move coordinate box geometries based on camera position. */
  NativeSceneLayer.prototype.moveCoordinateBox = function() {
    this.coordinateBox.move(this.camera);
  };

  /** Set near/far camera clip planes based on world bounding box. */
  NativeSceneLayer.prototype.setClipPlanes = function() {
    if (!(this.camera instanceof COMSOL.ComsolCamera))
      throw "Internal error";
    var cameraInverse = new THREE.Matrix4().getInverse(this.camera.matrix);
    this.scene.updateMatrixWorld();
    var worldBox = new THREE.Box3();
    var worldBoxC = new THREE.Box3();
    this.scene.traverse(function (node) {
      var b = node.geometry && node.geometry.boundingBox;
      if (b) {
        for (var xi = 0; xi < 2; xi++) {
          var xCrd = xi ? b.max.x : b.min.x;
          for (var yi = 0; yi < 2; yi++) {
            var yCrd = yi ? b.max.y : b.min.y;
            for (var zi = 0; zi < 2; zi++) {
              var zCrd = zi ? b.max.z : b.min.z;
              var p = new THREE.Vector3(xCrd, yCrd, zCrd);
              p.applyMatrix4(node.matrixWorld);
              worldBox.expandByPoint(p);
              p.applyMatrix4(cameraInverse);
              worldBoxC.expandByPoint(p);
            }
          }
        }
      }
    });
    var wbCenter = new THREE.Vector3((worldBoxC.min.x + worldBoxC.max.x) / 2,
                                     (worldBoxC.min.y + worldBoxC.max.y) / 2,
                                     (worldBoxC.min.z + worldBoxC.max.z) / 2);
    var wbHalfSize = new THREE.Vector3((worldBoxC.max.x - worldBoxC.min.x) / 2,
                                       (worldBoxC.max.y - worldBoxC.min.y) / 2,
                                       (worldBoxC.max.z - worldBoxC.min.z) / 2);
    var boundingRadius = worldBoxC.isEmpty() || (wbHalfSize.length() == 0) ? 1.0 : wbHalfSize.length() * 1.5;
    var dist = worldBoxC.isEmpty() ? 1.0 : Math.abs(wbCenter.z);
    var nearCP = Math.max(dist - boundingRadius, boundingRadius * 1e-3);
    var farCP = dist + boundingRadius;

    this.camera.near = nearCP;
    this.camera.far = farCP;
    this.camera.updateProjectionMatrix();
  };

  /** Compute bounding box for a BufferGeometry object. */
  NativeSceneLayer.computeBufferGeomBBox = function(geom) {
    var isSet = false;
    var box = new THREE.Box3();
    var pos = geom.getAttribute('position').array;
    if (geom.getIndex() !== null) {
      var idx = geom.getIndex().array;
      for (var j = 0; j < geom.groups.length; j++) {
        var start = geom.groups[j].start;
        var count = geom.groups[j].count;
        for (var i = 0; i < count; i++) {
          box.expandByPoint(new THREE.Vector3(pos[idx[start+i]*3],
              pos[idx[start+i]*3+1],
              pos[idx[start+i]*3+2]));
          isSet = true;
        }
      }
    }
    else {
      for (var i = 0; i < pos.length/3; i++) {
        box.expandByPoint(new THREE.Vector3(pos[i*3], pos[i*3+1],  pos[i*3+2]));
        isSet = true;
      }
    }

    geom.boundingBox = box;
    if (isSet) {
      var center = new THREE.Vector3((box.min.x + box.max.x) / 2,
                                     (box.min.y + box.max.y) / 2,
                                     (box.min.z + box.max.z) / 2);
      var size = new THREE.Vector3((box.max.x - box.min.x) / 2,
                                   (box.max.y - box.min.y) / 2,
                                   (box.max.z - box.min.z) / 2);
      geom.boundingSphere = new THREE.Sphere(center, size.length());
    }
  };

  return NativeSceneLayer;
})();

// ------------------------------------------------------------

/** Defines a scene including all layers. */
COMSOL.NativeScene = (function() {
  var NativeProtocol = COMSOL.NativeProtocol;
  var debugLog = COMSOL.Logger.isDebugEnabled();

  function NativeScene(canvas3D, useWebGL, context2D, setInitiatedCallback, sendCommand, targetId, w, h) {
    this.canvas3D = canvas3D;   // Canvas to use for 3D rendering
    this.renderer = null;       // three.js renderer object
    this.webGLSuspended = false;// True when WebGL context is lost
    this.mouseCtrl = null;      // MouseControl object
    this.useWebGL = useWebGL;   // True to use WebGL 3D rendering,
                                // false to use 2D image for 3D graphics
    this.context2D = context2D; // 2d context used for zoom/pan overlay
    this.isMode3D = true;       // True when in 3D mode, false when in 2D/1D mode.
    this.modeDefined = false;   // Set to true when SET_RENDERER command received.
    this.mouseActive = false;   // True during mouse actions
    this.zoomPanInfo = null;    // For zoom/pan visualization
    this.useLight = true;       // True to use scene light
    this.camera3DData = null;   // 3D camera data
    this.camera2DData = null;   // 2D camera data

    this.setInitiatedCallback = setInitiatedCallback;
    this.sendCommand = sendCommand; // Function that sends commands to the server
    this.targetId = targetId;
    this.width = w;
    this.height = h;

    // Scene definition using only COMSOL objects
    this.textures = {};        // id -> NativeTexture
    this.vertexBuffers = {};   // id -> NativeVertexBuffer
    this.indexBuffers = {};    // id -> NativeIndexBuffer
    this.sequences = {};       // id -> NativeRenderSequence
    this.sequenceIds = [];     // Uint16Array
    this.coordinateBox = null; // Coordinate box definition
    this.background = null;    // Background definition
    this.renderAttribute = new COMSOL.RenderAttribute();
    this.images = {};          // layerType -> NativeImage

    // Handle WebGL context lost/restored events
    var self = this;
    function webGLContextLost(event) {
      event.preventDefault();
      self.handleContextLost();
    }
    function webGLContextRestored(event) {
      self.handleContextRestored();
    }
    if (useWebGL) {
      canvas3D.addEventListener("webglcontextlost", webGLContextLost, false);
      canvas3D.addEventListener("webglcontextrestored", webGLContextRestored, false);
    }

    this.initRenderer();

    this.reportCapabilities();
    this.reportTargetDefinition();
    this.reportSize(w, h);
  }

  /** Initialize three.js renderer. */
  NativeScene.prototype.initRenderer = function() {
    // THREE texture objects
    this.threeTextures = {};   // id -> THREE.DataTexture

    // THREE scene/geom/material/mesh objects
    this.layers = {};  // Indexed by layer type
    function initLayer(scene, layerType) {
      if (scene.images[layerType] === undefined)
        scene.images[layerType] = new COMSOL.NativeImage(scene);
      scene.layers[layerType] = new COMSOL.NativeSceneLayer(scene, layerType,
                                                            scene.images[layerType]);
    }
    var rl = NativeProtocol.rl;
    initLayer(this, rl.BACKGROUND);
    initLayer(this, rl.SCENE);
    initLayer(this, rl.TRIAD);
    initLayer(this, rl.DECORATION);

    if (this.useWebGL) {
      this.renderer = new THREE.WebGLRenderer({devicePixelRatio: 1,
                                               canvas: this.canvas3D,
                                               antialias: true});
    } else {
      this.renderer = new THREE.CanvasRenderer({devicePixelRatio: 1,
                                                canvas: this.canvas3D});
    }
    this.renderer.setClearColor(new THREE.Color(0xEEEEEE), 1);
    this.renderer.setSize(this.width, this.height);
  }

  /** Called when WebGL context is lost. */
  NativeScene.prototype.handleContextLost = function() {
    this.webGLSuspended = true;
    this.renderer = null;
    this.renderScene();
  }

  /** Called when WebGL context is restored. */
  NativeScene.prototype.handleContextRestored = function() {
    this.initRenderer();
    this.setUseLight();
    if (this.isMode3D) {
      if (this.camera3DData)
        this.layers[NativeProtocol.rl.SCENE].setupCamera3D(this.camera3DData);
    } else {
      if (this.camera2DData)
        this.layers[NativeProtocol.rl.SCENE].setupCamera2D(this.camera2DData);
    }
    this.updateSceneObjects();
    this.webGLSuspended = false;
    this.renderScene();
  }

  /** Set mouse control object. */
  NativeScene.prototype.setMouseControl = function(mouseCtrl) {
    this.mouseCtrl = mouseCtrl;
  }

  /** Update scene state from a command. */
  NativeScene.prototype.handleCommand = function(binaryMessage) {
    var message = NativeProtocol.unpackMessage(binaryMessage);
    if ((message.target != 0) && (this.targetId != 0) &&
        (message.target != this.targetId))
      return; // Ignore messages with wrong target

    switch (message.msgType) {
    case NativeProtocol.mt.IMAGE_DATA:
      this.layers[message.layer].nativeImage.setImage(message.data);
      break;
    case NativeProtocol.mt.SCENE_FINISHED:
      this.updateSceneObjects();
      this.renderScene();
      break;
    case NativeProtocol.mt.SET_RENDERER:
      switch(message.renderer) {
      case NativeProtocol.rt.WEBRENDERER_GL_3D:
        this.isMode3D = true;
        break;
      case NativeProtocol.rt.WEBRENDERER_IMAGE_3D:
          this.isMode3D = true;
          break;
      case NativeProtocol.rt.WEBRENDERER_IMAGE_2D:
        this.isMode3D = false;
        break;
      default:
          throw "Invalid renderer type: " + message.renderer;
      }
      this.modeDefined = true;
      this.get3DCamera().setMode(this.isMode3D);
      break;
    case NativeProtocol.mt.OVERLAY_BOX_MODE:
      if (this.mouseCtrl)
        this.mouseCtrl.setOverlayBoxMode(message.enableOverlayBox != 0);
      break;
    case NativeProtocol.mt.TEXTURE_2D:
      if (this.textures[message.textureId])
        throw "Texture buffer already exists, id:" + message.textureId;
      this.textures[message.textureId] = new COMSOL.NativeTexture(message);
      break;
    case NativeProtocol.mt.SUBTEXTURE_2D:
      if (!this.textures[message.textureId])
        throw "Missing texture buffer, id:" + message.textureId;
      this.textures[message.textureId].setSubTexture(message);
      break;
    case NativeProtocol.mt.DELETE_TEXTURE:
      delete this.textures[message.textureId];
      break;
    case NativeProtocol.mt.CREATE_VERTEX_BUFFER:
      if (this.vertexBuffers[message.vertexBufferId])
        throw "Vertex buffer already exists, id:" + message.vertexBufferId;
      this.vertexBuffers[message.vertexBufferId] = new COMSOL.NativeVertexBuffer(message);
      break;
    case NativeProtocol.mt.FILL_VERTEX_BUFFER:
      if (!this.vertexBuffers[message.vertexBufferId])
        throw "Missing vertex buffer, id:" + message.vertexBufferId;
      this.vertexBuffers[message.vertexBufferId].fillBuffer(message);
      break;
    case NativeProtocol.mt.DELETE_VERTEX_BUFFER:
      delete this.vertexBuffers[message.vertexBufferId];
      break;
    case NativeProtocol.mt.CREATE_INDEX_BUFFER:
      if (this.indexBuffers[message.indexBufferId])
        throw "Index buffer already exists, id:" + message.indexBufferId;
      this.indexBuffers[message.indexBufferId] = new COMSOL.NativeIndexBuffer(message);
      break;
    case NativeProtocol.mt.FILL_INDEX_BUFFER:
      if (!this.indexBuffers[message.indexBufferId])
        throw "Missing index buffer, id:" + message.indexBufferId;
      this.indexBuffers[message.indexBufferId].fillBuffer(message);
      break;
    case NativeProtocol.mt.DELETE_INDEX_BUFFER:
      delete this.indexBuffers[message.indexBufferId];
      break;
    case NativeProtocol.mt.DEFINE_SEQUENCE:
      var seq = new COMSOL.NativeRenderSequence();
      for (var a = 0; a < message.actions.length; a++) {
        var ra = new COMSOL.NativeRenderAction(message.actions[a]);
        seq.actions.push(ra);
      }
      this.sequences[message.sequenceId] = seq;
      break;
    case NativeProtocol.mt.DELETE_SEQUENCE:
      delete this.sequences[message.sequenceId];
      break;
    case NativeProtocol.mt.DEFINE_SEQUENCE_ORDER:
      this.sequenceIds = message.sequenceIds;
      break;
    case NativeProtocol.mt.DEFINE_SUB_SEQUENCE:
      var seqList = new COMSOL.NativeSequenceList(message);
      this.sequences[message.sequenceId] = seqList;
      break;
    case NativeProtocol.mt.SETUP_CAMERA_3D:
      this.camera3DData = message;
      this.layers[NativeProtocol.rl.SCENE].setupCamera3D(this.camera3DData);
      break;
    case NativeProtocol.mt.SETUP_CAMERA_2D:
      this.camera2DData = message;
      this.layers[NativeProtocol.rl.SCENE].setupCamera2D(this.camera2DData);
      break;
    case NativeProtocol.mt.USE_LIGHT:
      this.useLight = message.useLight != 0;
      this.setUseLight();
      break;
    case NativeProtocol.mt.COORDINATE_BOX:
      this.coordinateBox = message;
      break;
    case NativeProtocol.mt.DEFINE_BACKGROUND:
      this.background = message;
      break;
    case NativeProtocol.mt.CONNECTION_INITIATED:
      this.setInitiatedCallback();
      break;
    default:
      throw "Invalid message type: " + message.msgType;
    }
  };

  /** Enable or disable scene light. */
  NativeScene.prototype.setUseLight = function() {
    this.layers[NativeProtocol.rl.DECORATION].useSceneLights(this.useLight);
    this.layers[NativeProtocol.rl.SCENE].useSceneLights(this.useLight);
  }

  /** Get the NativeRenderSequence objects corresponding to a sequence id. */
  NativeScene.prototype.getSequences = function(seqId) {
    var seq = this.sequences[seqId];
    var sequences = [];
    if (seq instanceof COMSOL.NativeRenderSequence) {
      sequences.push(seq);
    } else if (seq instanceof COMSOL.NativeSequenceList) {
      for (var i = 0; i < seq.sequenceIds.length; i++) {
        var subSeq = this.sequences[seq.sequenceIds[i]];
        sequences.push(subSeq);
      }
    } else {
      throw "Invalid sequence id: " + seqId;
    }
    return sequences;
  };

  /** Add/remove/update THREE scene to match current state. */
  NativeScene.prototype.updateSceneObjects = function() {
    var layer = -1;

    // Remove all render elements from all layers
    for (var sl in this.layers)
      this.layers[sl].renderElementsArray = [];

    var bgLayer = this.layers[NativeProtocol.rl.BACKGROUND];
    bgLayer.defineBackground(this.background);

    this.updateTextures();

    var sceneLayer = this.layers[NativeProtocol.rl.SCENE];
    sceneLayer.defineCoordinateBox(this.coordinateBox);

    /** Helper function to update attributes. */
    function handleAttribute(scene, act) {
      switch (act.actionType) {
      case NativeProtocol.ra.SET_ATTRIBUTE_POINT_SIZE:
        scene.renderAttribute.pointSize = act.pointSize;
        return true;
      case NativeProtocol.ra.SET_ATTRIBUTE_LINE_WIDTH:
        scene.renderAttribute.lineWidth = act.lineWidth;
        return true;
      case NativeProtocol.ra.SET_ATTRIBUTE_POLYGON_LAYER:
        scene.renderAttribute.polygonLayer = act.polygonLayer;
        return true;
      case NativeProtocol.ra.SET_ATTRIBUTE_CULL_FACE:
        scene.renderAttribute.cullface = act.cullface;
        return true;
      case NativeProtocol.ra.SET_ATTRIBUTE_COLOR:
        var b = ((act.color & 0x00ff0000) >>> 16) / 255;
        var g = ((act.color & 0x0000ff00) >>> 8 ) / 255;
        var r = ((act.color & 0x000000ff)       ) / 255;
        scene.renderAttribute.color = new THREE.Color(r, g, b);
        return true;
      case NativeProtocol.ra.SET_ATTRIBUTE_MATERIAL:
        var tp = act.materialType;
        if (tp == NativeProtocol.rmt.NONE) {
          scene.renderAttribute.material = undefined;
        } else {
          scene.renderAttribute.material = {};
          scene.renderAttribute.material.materialType = act.materialType;
            
          var b = ((act.ambientColor & 0x00ff0000) >>> 16) / 255;
          var g = ((act.ambientColor & 0x0000ff00) >>> 8 ) / 255;
          var r = ((act.ambientColor & 0x000000ff)       ) / 255;
          scene.renderAttribute.material.ambientColor = new THREE.Color(r, g, b);

          b = ((act.diffuseColor & 0x00ff0000) >>> 16) / 255;
          g = ((act.diffuseColor & 0x0000ff00) >>> 8 ) / 255;
          r = ((act.diffuseColor & 0x000000ff)       ) / 255;
          scene.renderAttribute.material.diffuseColor = new THREE.Color(r, g, b);

          b = ((act.specularColor & 0x00ff0000) >>> 16) / 255;
          g = ((act.specularColor & 0x0000ff00) >>> 8 ) / 255;
          r = ((act.specularColor & 0x000000ff)       ) / 255;
          scene.renderAttribute.material.specularColor = new THREE.Color(r, g, b);            
        }
        return true;
      case NativeProtocol.ra.SET_ATTRIBUTE_TRANSPARENCY:
        scene.renderAttribute.transparency = act.transparency;
        return true;
      default:
        return false;
      }
    }

    // Handle attributes set on sequences used only by the coordinate box
    var cb = this.coordinateBox;
    if (cb && this.isMode3D) {
      var boxIds = [ cb.labelsX1SeqId, cb.labelsY1SeqId, cb.labelsZ1SeqId,
                     cb.labelsX2SeqId, cb.labelsY2SeqId, cb.labelsZ2SeqId ];
      for (var i = 0; i < boxIds.length; i++) {
        if (boxIds[i]) {
          var sequences = this.getSequences(boxIds[i]);
          for (var seqIdx = 0; seqIdx < sequences.length; seqIdx++) {
            var seq = sequences[seqIdx];
            for (var j = 0; j < seq.actions.length; j++)
              handleAttribute(this, seq.actions[j]);
          }
        }
      }
    }

    // Add render elements to all layers
    for (var i = 0; i < this.sequenceIds.length; i++) {
      var sequences = this.getSequences(this.sequenceIds[i]);
      for (var seqIdx = 0; seqIdx < sequences.length; seqIdx++) {
        var seq = sequences[seqIdx];
        var seqRenderState = new COMSOL.RenderState();
        for (var j = 0; j < seq.actions.length; j++) {
          var act = seq.actions[j];
          switch (act.actionType) {
          case NativeProtocol.ra.RENDER_INDEXED_ELEMENTS:
          case NativeProtocol.ra.RENDER_VERTEXED_ELEMENTS:
          case NativeProtocol.ra.RENDER_LABEL_3D:
          case NativeProtocol.ra.RENDER_LABEL_3D_2:
            var sceneLayer = this.layers[layer];
            if (sceneLayer === undefined)
              throw "Invalid layer: " + layer;
            var id = seq.objectId + "_" + j;
            var nre = new COMSOL.NativeRenderElements(act, id, this.renderAttribute, seqRenderState);
            sceneLayer.renderElementsArray.push(nre);
            break;
          case NativeProtocol.ra.SET_RENDERING_LAYER:
            layer = act.layerType;
            break;
          case NativeProtocol.ra.SET_STATE_MODEL_TRANSFORMATION:
            seqRenderState.modelTransformation = new THREE.Matrix4().set(
              act.m11, act.m12, act.m13, act.m14,
              act.m21, act.m22, act.m23, act.m24,
              act.m31, act.m32, act.m33, act.m34,
              act.m41, act.m42, act.m43, act.m44
            );
            break;
          case NativeProtocol.ra.SET_STATE_DEPTH_TEST:
            seqRenderState.depthTest = act.depthTest;
            break;
          case NativeProtocol.ra.SET_STATE_TEXTURE_TRANSFORMATION:
            // skipping the `act.textureUnit` since it is always 0
            seqRenderState.textureTransformation = new THREE.Matrix4().set(
                act.m11, act.m12, act.m13, act.m14,
                act.m21, act.m22, act.m23, act.m24,
                act.m31, act.m32, act.m33, act.m34,
                act.m41, act.m42, act.m43, act.m44
            );
            break;
          default:
            if (!handleAttribute(this, act))
              throw "Unknown render action type: " + act.actionType;
          }
        }
      }
    }

    for (var sl in this.layers)
      this.layers[sl].updateScene();

    this.resetModified();
  };

  /** Add/remove/update THREE texture objects. */
  NativeScene.prototype.updateTextures = function() {
    // Delete no longer used textures
    var newThreeTextures = {};
    for (var t in this.threeTextures)
      if (this.textures[t])
        newThreeTextures[t] = this.threeTextures[t];
    this.threeTextures = newThreeTextures;

    // Add new and update modified textures
    for (var t in this.textures) {
      var ct = this.textures[t];
      if (!this.threeTextures[t] || ct.modified) {
        var format;
        switch (ct.format) {
        case NativeProtocol.tfmt.GL_ALPHA:
          format = THREE.AlphaFormat;
          break;
        case NativeProtocol.tfmt.GL_RGB:
          format = THREE.RGBFormat;
          break;
        case NativeProtocol.tfmt.GL_RGBA:
          format = THREE.RGBAFormat;
          break;
        case NativeProtocol.tfmt.GL_LUMINANCE:
          format = THREE.LuminanceFormat;
          break;
        case NativeProtocol.tfmt.GL_LUMINANCE_ALPHA:
          format = THREE.LuminanceAlphaFormat;
          break;
        default:
          throw "Unsupported texture format: " + ct.format;
        }
        var tt;
        if (this.useWebGL) {
          tt = new THREE.DataTexture(ct.pixelData, ct.width, ct.height, format);
        } else {
          // CanvasRenderer does not support DataTexture, use image based texture
          tt = new THREE.Texture(ct.createCanvas());
        }
        tt.generateMipmaps = false;
        tt.minFilter = THREE.LinearFilter;
        tt.magFilter = THREE.LinearFilter;
        tt.needsUpdate = true;
        switch (ct.width % 4) {
        case 1: case 3:
          tt.unpackAlignment = 1;
          break;
        case 2:
          tt.unpackAlignment = 2;
          break;
        default:
          tt.unpackAlignment = 4;
          break;
        }
        tt.flipY = false;
        this.threeTextures[t] = tt;
      }
    }
  };

  /** Reset all modified flags. */
  NativeScene.prototype.resetModified = function() {
    for (var t in this.textures)
      this.textures[t].modified = false;

    for (var v in this.vertexBuffers) {
      var vb = this.vertexBuffers[v];
      for (var s in vb.subBuffers)
        vb.subBuffers[s].modified = false;
    }

    for (var i in this.indexBuffers) {
      var ib = this.indexBuffers[i];
      for (var s = 0; s < ib.subBuffersModified.length; s++)
        ib.subBuffersModified[s] = false;
    }
  };

  /** Render all layers. */
  NativeScene.prototype.renderScene = function() {
    var layer, rl = NativeProtocol.rl;

    if (!this.modeDefined) {
      if (this.useWebGL)
        this.canvas3D.style.visibility = "hidden";
      return;
    }

    if (this.webGLSuspended && this.isMode3D) {
      this.canvas3D.style.visibility = "hidden";
      return;
    }

    // Update triad camera
    var camScene = this.layers[rl.SCENE].camera;
    var camTriad = this.layers[rl.TRIAD].camera;
    var posVec = (new THREE.Vector3(0,0,1)).applyMatrix4(camScene.matrix);
    var upVec4 = (new THREE.Vector4(0,1,0,0)).applyMatrix4(camScene.matrix);
    camScene.updateMatrix();
    camTriad.position.set(posVec.x, posVec.y, posVec.z);
    camTriad.up.set(upVec4.x, upVec4.y, upVec4.z);
    camTriad.lookAt(new THREE.Vector3(0,0,0));

    var ctx = this.context2D;
    var zi = this.zoomPanInfo;
    var w = this.width;
    var h = this.height;

    // Hide WebGL canvas when 2D canvas is used
    if (this.useWebGL) {
      this.canvas3D.style.visibility = this.isMode3D ? "visible" : "hidden";
      var overlayBox = this.mouseActive && zi && (zi.overlayBoxEndX != -1) && (zi.overlayBoxEndY != -1);
      ctx.canvas.style.visibility = (this.isMode3D && !overlayBox) ? "hidden" : "visible";
    }

    ctx.save();
    try {
      var renderer = this.renderer;
      // Render all layers
      if (renderer) {
        renderer.autoClear = true;
        renderer.autoClearColor = true;
        renderer.autoClearDepth = true;
      }
      ctx.globalAlpha = 1;

      layer = this.layers[rl.BACKGROUND]
      if (this.useWebGL && this.isMode3D) {
        renderer.sortObjects = false;
        layer.transformLabels();
        renderer.render(layer.scene, layer.camera);
      } else {
        ctx.clearRect(0, 0, w, h);
        layer.nativeImage.draw(ctx, w, h);
      }

      layer = this.layers[rl.SCENE];
      layer.moveCoordinateBox();
      layer.transformLabels();
      layer.setClipPlanes();
      layer.transformLights();
      if (this.useWebGL && this.isMode3D) {
        renderer.autoClearColor = false;
        renderer.autoClearDepth = false;
        renderer.sortObjects = false;
        renderer.render(layer.scene, layer.camera);
      } else {
        if (renderer)
          renderer.autoClear = false;
        if (this.isMode3D) {
          renderer.render(layer.scene, layer.camera);
        }
        layer.nativeImage.draw(ctx, w, h);
      }

      if (this.isMode3D) {
        layer = this.layers[rl.TRIAD];
        if (this.useWebGL) {
          renderer.autoClearColor = false;
          renderer.autoClearDepth = true;
        } else {
          renderer.autoClear = false;
        }
        renderer.sortObjects = true;
        layer.transformLabels();
        renderer.render(layer.scene, layer.camera);
      }

      layer = this.layers[rl.DECORATION];
      if (this.useWebGL && this.isMode3D) {
        renderer.autoClearColor = false;
        renderer.autoClearDepth = false;
        renderer.sortObjects = false;
        layer.transformLabels();
        renderer.render(layer.scene, layer.camera);
      } else {
        layer.nativeImage.draw(ctx, w, h);
      }

      // Render zoom/pan visualization
      if (this.mouseActive && zi) {
        ctx.lineCap = "square";
        var camera = this.layers[rl.SCENE].camera;
        if (zi.overlayBoxEndX != -1 && zi.overlayBoxEndY != -1) { // Overlay box
          if (this.useWebGL && this.isMode3D)
            ctx.clearRect(0, 0, w, h);
          ctx.strokeStyle = "rgba(101,101,248,1)";
          ctx.lineWidth = 1;
          var x0 = zi.startX * w;
          var y0 = (1 -zi.startY) * h;
          var x1 = zi.overlayBoxEndX * w;
          var y1 = (1 - zi.overlayBoxEndY) * h;
          ctx.strokeRect(Math.min(x0, x1), Math.min(y0, y1),
                         Math.abs(x1 - x0), Math.abs(y1 - y0));
        } else if (!this.isMode3D) { // Grid lines
          ctx.strokeStyle = "rgba(101,101,248,0.4)";
          if (zi.panX || zi.panY) {
            ctx.translate(zi.panX * w, -zi.panY * h);
          } else {
            ctx.translate(zi.startX * w, (1-zi.startY) * h);
            ctx.scale(zi.zoom, zi.zoom);
            ctx.translate(-zi.startX * w, -(1-zi.startY) * h);
          }
          ctx.lineWidth = 2 * (zi.zoom > 1 ? 1/zi.zoom : 1/Math.sqrt(zi.zoom));

          var N = 8;
          ctx.beginPath();
          var minX = camera.drawMinX / w;
          var maxX = camera.drawMaxX / w;
          var minY = 1 - camera.drawMaxY / h;
          var maxY = 1 - camera.drawMinY / h;
          for (var i = 0; i <= N; i++) {
            var k = i / N;
            ctx.moveTo(minX*w, (minY+k*(maxY-minY))*h);
            ctx.lineTo(maxX*w, (minY+k*(maxY-minY))*h);
            ctx.moveTo((minX+k*(maxX-minX))*w, minY*h);
            ctx.lineTo((minX+k*(maxX-minX))*w, maxY*h);
          }
          ctx.stroke();
        }
      }
    } finally {
      ctx.restore();
    }
  };

  /** Return the camera used to render the 3D scene. */
  NativeScene.prototype.get3DCamera = function() {
    return this.layers[NativeProtocol.rl.SCENE].camera;
  };

  /** Called when window is resized. */
  NativeScene.prototype.onResize = function(w, h) {
    if (this.renderer)
      this.renderer.setSize(w, h);
    if (this.useWebGL) {
      this.context2D.canvas.width = w;
      this.context2D.canvas.height = h;
    }

    this.width = w;
    this.height = h;
    for (var sl in this.layers)
      this.layers[sl].onResize(w, h);
    this.renderScene();
    this.reportSize(w, h);
  };

  /** Report capabilities to server. */
  NativeScene.prototype.reportCapabilities = function() {
    var binCmd = new Uint16Array(8);
    var i = 0;
    binCmd[i++] = 0x0000; // Target, always 0
    binCmd[i++] = 0x9100; // Renderer capabilities
    var dpi = COMSOL.BrowserInfo.getDPI();
    binCmd[i++] = dpi & 0xffff;
    binCmd[i++] = (dpi >>> 16) & 0xffff;
    var flags = 0;
    var maxTextureSize = 4096;
    if (this.useWebGL) {
      flags |= 0x1; // WebGL supported
      maxTextureSize = COMSOL.BrowserInfo.getMaxTextureSize(this.renderer);
    }
    binCmd[i++] = flags & 0xffff;
    binCmd[i++] = (flags >>> 16) & 0xffff;
    binCmd[i++] = maxTextureSize & 0xffff;
    binCmd[i++] = (maxTextureSize >>> 16) & 0xffff;

    if (debugLog) {
      var txt = "<- target:0 msgType:RENDER_CAPABILITIES dpi:" + dpi +
                " flags:" + flags + " maxTextureSize:" + maxTextureSize;
      COMSOL.Logger.debug(txt);
    }
    this.sendCommand({attachments: [binCmd]}, true);
  };

  /** Report target axis to target id mapping to server. */
  NativeScene.prototype.reportTargetDefinition = function() {
    var params = COMSOL.getQueryParams();
    var targetAxis = params["targetaxis"] || "";
    var binCmd = new Uint8Array(8 + targetAxis.length);
    var i = 0;
    binCmd[i++] = 0;     // Target, always 0
    binCmd[i++] = 0;
    binCmd[i++] = 0x01;  // Target definition, 0x9101
    binCmd[i++] = 0x91;
    binCmd[i++] = this.targetId & 0xff;
    binCmd[i++] = (this.targetId >>> 8) & 0xff;
    binCmd[i++] = targetAxis.length & 0xff;
    binCmd[i++] = (targetAxis.length >>> 8) & 0xff;
    for (var j = 0; j < targetAxis.length; j++)
      binCmd[i++] = targetAxis.charCodeAt(j);
    if (debugLog) {
      var txt = "<- target:0 msgType:TARGET_DEFINITION id:" + this.targetId +
                " targetAxis:" + targetAxis;
      COMSOL.Logger.debug(txt);
    }
    this.sendCommand({attachments: [binCmd]}, true);
  };

  /** Report window size to server. */
  NativeScene.prototype.reportSize = function(w, h) {
    var binCmd = new Uint16Array(4);
    binCmd[0] = this.targetId;
    binCmd[1] = 0x8002; // Canvas size
    binCmd[2] = w;      // Width
    binCmd[3] = h;      // Height
    if (debugLog) {
      var txt = "<- target:" + this.targetId + " msgType:CANVAS_SIZE width:" + w + " height:" + h;
      COMSOL.Logger.debug(txt);
    }
    this.sendCommand({attachments: [binCmd]}, false);
  };

  /** Report current camera settings to server. */
  NativeScene.prototype.reportCameraSettings = function() {
    var camera = this.layers[NativeProtocol.rl.SCENE].camera;
    if (this.isMode3D) {
      var binCmd = new ArrayBuffer(4+16*4);
      var buf = new DataView(binCmd);
      buf.setInt16(0, this.targetId, true);
      buf.setInt16(2, 0x8004, true); // ViewSettings3D
      var i = 1;
      var isPerspective = camera.isPerspectiveProjection();
      buf.setInt32((i++)*4, camera.projectionType, true);
      if (isPerspective)
        buf.setFloat32((i++)*4, camera.fov / 2, true);
      else
        buf.setFloat32((i++)*4, camera.orthoScale, true);
      buf.setFloat32((i++)*4, camera.position.x, true);
      buf.setFloat32((i++)*4, camera.position.y, true);
      buf.setFloat32((i++)*4, camera.position.z, true);
      buf.setFloat32((i++)*4, camera.target.x, true);
      buf.setFloat32((i++)*4, camera.target.y, true);
      buf.setFloat32((i++)*4, camera.target.z, true);
      buf.setFloat32((i++)*4, camera.up.x, true);
      buf.setFloat32((i++)*4, camera.up.y, true);
      buf.setFloat32((i++)*4, camera.up.z, true);
      buf.setFloat32((i++)*4, camera.rotationPoint.x, true);
      buf.setFloat32((i++)*4, camera.rotationPoint.y, true);
      buf.setFloat32((i++)*4, camera.rotationPoint.z, true);
      buf.setFloat32((i++)*4, camera.nearRelDeltaX, true);
      buf.setFloat32((i++)*4, camera.nearRelDeltaY, true);

      if (debugLog) {
        var txt = "<- target:" + this.targetId + " msgType:VIEW_SETTINGS_3D projectionType:" +
          camera.projectionType + " zoom:" + (isPerspective ? camera.fov/2 : camera.orthoScale) +
          " position:" + camera.position.x + " " + camera.position.y + " " + camera.position.z +
          " target:" + camera.target.x + " " + camera.target.y + " " + camera.target.z +
          " up:" + camera.up.x + " " + camera.up.y + " " + camera.up.z +
          " centerRot:" + camera.rotationPoint.x + " " + camera.rotationPoint.y +
          " " + camera.rotationPoint.z +
          " viewOffset:" + camera.nearRelDeltaX + " " + camera.nearRelDeltaY;
        COMSOL.Logger.debug(txt);
      }
      this.sendCommand({attachments: [binCmd]}, false);
    } else {
      var binCmd = new ArrayBuffer(4+4*4);
      var buf = new DataView(binCmd);
      buf.setInt16(0, this.targetId, true);
      buf.setInt16(2, 0x8006, true); // ViewSettings2D
      var i = 1;
      buf.setFloat32((i++)*4, camera.xAxisMin, true);
      buf.setFloat32((i++)*4, camera.xAxisMax, true);
      buf.setFloat32((i++)*4, camera.yAxisMin, true);
      buf.setFloat32((i++)*4, camera.yAxisMax, true);

      if (debugLog) {
        var txt = "<- target:" + this.targetId + " msgType:VIEW_SETTINGS_2D" +
          " minX:" + camera.xAxisMin + " maxX:" + camera.xAxisMax +
          " minY:" + camera.yAxisMin + " maxY:" + camera.yAxisMax;
        COMSOL.Logger.debug(txt);
      }
      this.sendCommand({attachments: [binCmd]}, false);
    }
  };

  /** Report mouse event to server. */
  NativeScene.prototype.reportMouseEvent = function(eventType, event, wheelDelta) {
    var binCmd = new ArrayBuffer(4+2*2+3*4);
    var buf = new DataView(binCmd);
    buf.setInt16(0, this.targetId, true);
    buf.setInt16(2, 0x8010, true); // MouseEvent
    buf.setInt16(4, eventType, true);
    if(eventType == 4) {
      buf.setInt16(6, wheelDelta, true);
    } else {
      buf.setInt16(6, event.button+1, true);
    }
    var modifiers = (event.altKey? 0x0001 : 0) | (event.shiftKey? 0x0002 : 0) | (event.ctrlKey? 0x0004 : 0);
    buf.setInt32(8, modifiers, true);
    buf.setInt32(12, event.clientX, true);
    buf.setInt32(16, event.clientY, true);

    if (debugLog) {
      var txt = "<- target:" + this.targetId + " msgType:MOUSE_EVENT" +
        " type:" + eventType +
        ((eventType == 4) ? " wheel:" + wheelDelta : " button:" + event.button) +
        " modifiers:" + modifiers + " X:" + event.clientX + " Y:" + event.clientY;
      COMSOL.Logger.debug(txt);
    }
    this.sendCommand({attachments: [binCmd]}, false);
  }

  /** Show/hide mouse action visualization. */
  NativeScene.prototype.setMouseAction = function(mouseActive, zoomPanInfo) {
    if (this.mouseActive != mouseActive) {
      this.mouseActive = mouseActive;
      if (mouseActive) {
        this.zoomPanInfo = zoomPanInfo;
      } else {
        this.zoomPanInfo = null;
        var sceneLayer = this.layers[NativeProtocol.rl.SCENE];
        if (!this.isMode3D) {
          if (this.useWebGL)
            this.context2D.clearRect(0, 0, this.width, this.height);
        }
        if(sceneLayer.camera.applyZoomPanInfo(zoomPanInfo)) {
          this.reportCameraSettings();
        }
      }
      this.renderScene();
    }
  };

  return NativeScene;
})();
