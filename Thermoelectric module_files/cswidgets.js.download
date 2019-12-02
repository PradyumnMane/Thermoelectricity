/**
 * Copyright (c) 1998-2016 by COMSOL AB
 * $Revision: $  $Date: $
 */
"use strict";

/** RWT client objects for customized widgets (e.g. BigButton). */
(function() {

  /**
   * Lookup for the possible states of a widget to the corresponding CSS classes
   * used to style widgets with that state.
   */
  var STYLES = {
    normal : null,
    disabled : 'disabled',
    pressed : 'pressed',
    selected : 'selected',
    hover : 'mouseover'
  };

  /** 
   * Base constructor for all customized widgets.
   * 
   * @param widgetType {String} The identifying type of the widget (used for CSS styling).
   */
  function CsWidget(widgetType) {
    this.widgetType = widgetType;
  }

  CsWidget.prototype = {
    setId : function(id) {
      this.id = id;
    },
    getId : function() {
      return this.id;
    },
    setTheme : function(theme) {
      this.theme = theme;
    },
    getTheme : function() {
      return this.theme;
    },
    getWidgetType : function() {
      return this.widgetType;
    },
    hasWidget: function() {
      return this.getWidget() !== undefined;
    },
    getWidget: function() {
      return rap.getObject(this.getId());
    },
    getElement: function() {
      return this.getWidget().$el.get()[0];
    }
  };

  /** Base constructor for all buttons. */
  function CsButton() {
    CsWidget.call(this, 'csbutton');
    this.currentStateStyle = STYLES.normal;
    this.hasCustomBackgroundColor = false;
  }

  CsButton.prototype = Object.create(CsWidget.prototype);
  CsButton.prototype.constructor = CsButton;

  CsButton.prototype.setSelected = function(selected) {
    this.selected = selected;
  };

  CsButton.prototype.getSelected = function() {
    return this.selected;
  };

  CsButton.prototype.setHasCustomBackgroundColor = function(hasCustomBackgroundColor) {
    this.hasCustomBackgroundColor = hasCustomBackgroundColor;
  };

  CsButton.prototype.getHasCustomBackgroundColor = function() {
    return this.hasCustomBackgroundColor;
  };

  CsButton.prototype.resetBaseStyle = function() {
    var widgetType = this.getWidgetType();
    var theme = this.getTheme();
    var buttonStyleMode = this.getButtonStyleMode();
    var backgroundMode = this.getHasCustomBackgroundColor() ? 'custom-bg' : 'default-bg';
    this.getElement().className = [widgetType, theme, buttonStyleMode, backgroundMode].join(' ');
  };

  CsButton.prototype.updateStateStyle = function(stateStyle) {
    if (this.currentStateStyle !== null) {
      this.getElement().classList.remove(this.currentStateStyle);
    }
    if (stateStyle !== null) {
      this.getElement().classList.add(stateStyle);
    }
    this.currentStateStyle = stateStyle;
  };

  CsButton.prototype.init = function() {
    var self = this;
    var widget = self.getWidget();
    widget.addListener('Show', function() {
      self.resetBaseStyle();
      self.updateStateStyle(self.getWidget().getEnabled() ? STYLES.normal : STYLES.disabled);
    });
    widget.addListener('MouseDown', function() {
      if (!self.getSelected()) {
        self.updateStateStyle(STYLES.pressed);
      }
    });
    widget.addListener('MouseUp', function() {
      if (!self.getSelected()) {
        self.updateStateStyle(STYLES.hover);
      }
    });
    widget.addListener('MouseExit', function() {
      self.updateStateStyle(self.getSelected() ? STYLES.selected : STYLES.normal);
    });
    widget.addListener('MouseEnter', function() {
      self.updateStateStyle(STYLES.hover);
    });
  };

  /** Callback invoked server-side when the state of the button has changed. */
  CsButton.prototype.notifyStateChange = function(data) {
    this.setSelected(data.selected);
    var style = data.enabled ? (data.selected ? STYLES.selected : STYLES.normal) : STYLES.disabled;
    if (this.hasWidget()) {
      this.updateStateStyle(style);
    }
  };

  /** Callback invoked server-side when the background of the button has changed. */
  CsButton.prototype.notifyBackgroundChange = function(data) {
    this.setHasCustomBackgroundColor(data.hasCustomBackgroundColor);
    if (this.hasWidget()) {
      this.resetBaseStyle();
    }
  };

  /** Base constructor for a 'big style' button. */
  function BigButton() {
    CsButton.call(this);
  }

  BigButton.prototype = Object.create(CsButton.prototype);
  BigButton.prototype.constructor = BigButton;

  BigButton.prototype.getButtonStyleMode = function() {
    return this.getTransparent() ? 'big-transparent' : 'big';
  };

  BigButton.prototype.setTransparent = function(transparent) {
    this.transparent = transparent;
  };

  BigButton.prototype.getTransparent = function() {
    return this.transparent;
  };

  /** Base constructor for a 'standard style' button. */
  function StandardButton() {
    CsButton.call(this);
  }

  StandardButton.prototype = Object.create(CsButton.prototype);
  StandardButton.prototype.constructor = StandardButton;

  StandardButton.prototype.getButtonStyleMode = function() {
    return 'standard';
  };

  function bigButtonFactory(properties) {
    return new BigButton();
  }

  function standardButtonFactory(properties) {
    return new StandardButton();
  }

  /**
   * Register a new custom COMSOL widget with RAP.
   * 
   * @param type {String}
   *          Identifier for the widget type. A corresponding string is
   *          used server-side to register the remote object type.
   * @param factory {Function}
   *          A factory method for constructing the widget.
   * @param properties {Array}
   *          A string array of widget properties that can be set
   *          server-side, i.e. a pair of methods named in the form setProperty
   *          and getProperty (for which the corresponding array element is
   *          'property').
   * @param methods {Array}
   *          A string array of widget methods that can be invoked
   *          server-side.
   */
  function registerTypeHandler(type, factory, properties, methods) {
    rap.registerTypeHandler(type, { factory : factory, properties : properties, methods : methods });
  }

  registerTypeHandler("com.comsol.webguiclient.bigbutton", bigButtonFactory, [ "id", "theme", "transparent" ], [ "init", "notifyStateChange", "notifyBackgroundChange" ]);
  registerTypeHandler("com.comsol.webguiclient.standardbutton", standardButtonFactory, [ "id", "theme" ], [ "init", "notifyStateChange", "notifyBackgroundChange" ]);

})();