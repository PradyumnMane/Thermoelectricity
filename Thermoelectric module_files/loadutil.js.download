/**
 * Copyright (c) 1998-2015 by COMSOL AB
 * $Revision: $  $Date: $
 */
"use strict";

/**
 * This script is loaded statically and contains helper methods for loading other
 * scripts dynamically (with correct version). If this file is changed, ensuring
 * that old versions of this file are not cached must be done manually (eg. adding version
 * number to script name).
 */

var COMSOL = COMSOL || {};

/** Get URL query parameters. */
COMSOL.getQueryParams = function() {
  var paramStrings = window.location.search.substring(1).split("&");
  var params = {};
  for (var i = 0; i < paramStrings.length; i++) {
    var keyAndVal = paramStrings[i].split("=");
    if (keyAndVal.length == 2) {
      var key = decodeURIComponent(keyAndVal[0]);
      var val = decodeURIComponent(keyAndVal[1]);
      params[key] = val;
    }
  }
  return params;
};

/** 
 * Initiate loading of a script with source 'script',
 * instruct the browser to load the script synchronously 
 */
COMSOL.loadScriptSync = function(doc, script) {
  var scriptEl = doc.createElement("script");
  scriptEl.src = script;
  scriptEl.async = false;
  doc.body.appendChild(scriptEl);
}
