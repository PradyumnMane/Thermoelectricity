/**
 * Copyright (c) 1998-2015 by COMSOL AB
 * $Revision: $  $Date: $
 */

"use strict";

/** get the styled nav */
(function() {
  var xhr = new XMLHttpRequest();
  var url = "../iframe-nav?relPath=../";

  function requestCallback() {
    if (this.status !== 200) {
      return;
    }
    var nav = document.getElementById('nav-holder');
    nav.innerHTML =  this.responseText;
  }

  xhr.addEventListener("load", requestCallback);
  xhr.open("GET", url);
  xhr.send();
})();

/** Set application loading text (with correct locale received from server). */
(function() {

  var xhr = new XMLHttpRequest();
  var url = "applicationstartup";
  
  function requestCallback() {
    var loadingText = document.getElementById('loading-application-text');
    var result;
    try {
      result = JSON.parse(this.responseText);
    } catch (e) {
      console.error("Could not parse JSON from responseText: " + e.message);
      throw e;
    }
    document.title = result.applicationLoadingText;
    loadingText.textContent = result.applicationLoadingText;
  }

  xhr.addEventListener("load", requestCallback);
  xhr.open("GET", url);
  xhr.send();
})();

/** get design pattern */
(function() {
  var pulse = document.getElementById('pulse-img');
  var img = new Image();
  img.onload = function () {
    pulse.src = img.src;
  };
  img.src = "../designpattern.svg";
})();
