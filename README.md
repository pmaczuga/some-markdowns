# Markdown

Here is html code
---

<html>
  <head>
    
<link rel="stylesheet" href="https://cdn.pydata.org/bokeh/release/bokeh-1.1.0.min.css">
<link rel="stylesheet" href="https://cdn.pydata.org/bokeh/release/bokeh-widgets-1.1.0.min.css">
<link rel="stylesheet" href="https://cdn.pydata.org/bokeh/release/bokeh-tables-1.1.0.min.css">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
<link rel="stylesheet" href="https://code.jquery.com/ui/1.10.4/themes/smoothness/jquery-ui.css">
<style>div.hololayout {
  display: flex;
  align-items: center;
  margin: 0;
}

div.holoframe {
  width: 75%;
}

div.holowell {
  display: flex;
  align-items: center;
}

form.holoform {
  background-color: #fafafa;
  border-radius: 5px;
  overflow: hidden;
  padding-left: 0.8em;
  padding-right: 0.8em;
  padding-top: 0.4em;
  padding-bottom: 0.4em;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
  margin-bottom: 20px;
  border: 1px solid #e3e3e3;
}

div.holowidgets {
  padding-right: 0;
  width: 25%;
}

div.holoslider {
  min-height: 0 !important;
  height: 0.8em;
  width: 100%;
}

div.holoformgroup {
  padding-top: 0.5em;
  margin-bottom: 0.5em;
}

div.hologroup {
  padding-left: 0;
  padding-right: 0.8em;
  width: 100%;
}

.holoselect {
  width: 92%;
  margin-left: 0;
  margin-right: 0;
}

.holotext {
  padding-left:  0.5em;
  padding-right: 0;
  width: 100%;
}

.holowidgets .ui-resizable-se {
  visibility: hidden
}

.holoframe > .ui-resizable-se {
  visibility: hidden
}

.holowidgets .ui-resizable-s {
  visibility: hidden
}


/* CSS rules for noUISlider based slider used by JupyterLab extension  */

.noUi-handle {
  width: 20px !important;
  height: 20px !important;
  left: -5px !important;
  top: -5px !important;
}

.noUi-handle:before, .noUi-handle:after {
  visibility: hidden;
  height: 0px;
}

.noUi-target {
  margin-left: 0.5em;
  margin-right: 0.5em;
}

div.bk-hbox {
    display: flex;
    justify-content: center;
}

div.bk-hbox div.bk-plot {
    padding: 8px;
}

div.bk-hbox div.bk-data-table {
    padding: 20px;
}
</style>
    
<script src="https://cdn.pydata.org/bokeh/release/bokeh-1.1.0.min.js" type="text/javascript"></script>
<script src="https://cdn.pydata.org/bokeh/release/bokeh-widgets-1.1.0.min.js" type="text/javascript"></script>
<script src="https://cdn.pydata.org/bokeh/release/bokeh-tables-1.1.0.min.js" type="text/javascript"></script>
<script src="https://cdn.pydata.org/bokeh/release/bokeh-gl-1.1.0.min.js" type="text/javascript"></script>
<script src="https://code.jquery.com/jquery-2.1.4.min.js" type="text/javascript"></script>
<script src="https://code.jquery.com/ui/1.10.4/jquery-ui.min.js" type="text/javascript"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.20/require.min.js" type="text/javascript"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.8.3/underscore-min.js" type="text/javascript"></script>
<script type="text/javascript">function HoloViewsWidget() {
}

HoloViewsWidget.prototype.init_slider = function(init_val){
  if(this.load_json) {
    this.from_json()
  } else {
    this.update_cache();
  }
}

HoloViewsWidget.prototype.populate_cache = function(idx){
  this.cache[idx].innerHTML = this.frames[idx];
  if (this.embed) {
    delete this.frames[idx];
  }
}

HoloViewsWidget.prototype.process_error = function(msg){
}

HoloViewsWidget.prototype.from_json = function() {
  var data_url = this.json_path + this.id + '.json';
  $.getJSON(data_url, $.proxy(function(json_data) {
    this.frames = json_data;
    this.update_cache();
    this.update(0);
  }, this));
}

HoloViewsWidget.prototype.dynamic_update = function(current){
  if (current === undefined) {
    return
  }
  this.current = current;
  if (this.comm) {
    var msg = {comm_id: this.id+'_client', content: current}
    this.comm.send(msg);
  }
}

HoloViewsWidget.prototype.update_cache = function(force){
  var frame_len = Object.keys(this.frames).length;
  for (var i=0; i<frame_len; i++) {
    if(!this.load_json || this.dynamic)  {
      var frame = Object.keys(this.frames)[i];
    } else {
      var frame = i;
    }
    if(!(frame in this.cache) || force) {
      if ((frame in this.cache) && force) { this.cache[frame].remove() }
      var div = document.createElement("div");
      var parent = document.getElementById("_anim_img"+this.id);
      div.style.display = "none";
      parent.appendChild(div)
      this.cache[frame] = div;
      var cache_id = "_anim_img"+this.id+"_"+frame;
      this.populate_cache(frame);
    }
  }
}

HoloViewsWidget.prototype.update = function(current){
  if(current in this.cache) {
    for (var index in this.cache) {
      this.cache[index].style.display = "none";
    }
    this.cache[current].style.display = "";
    this.wait = false;
  }
}

HoloViewsWidget.prototype.init_comms = function() {
  var that = this
  HoloViews.comm_manager.register_target(this.plot_id, this.id, function (msg) { that.msg_handler(msg) })
  if (!this.cached || this.dynamic) {
    function ack_callback(msg) {
      var msg = msg.metadata;
      var comm_id = msg.comm_id;
      var comm_status = HoloViews.comm_status[comm_id];
      if (that.queue.length > 0) {
        that.time = Date.now();
        that.dynamic_update(that.queue[that.queue.length-1]);
        that.queue = [];
      } else {
        that.wait = false;
      }
      if ((msg.msg_type == "Ready") && msg.content) {
        console.log("Python callback returned following output:", msg.content);
      } else if (msg.msg_type == "Error") {
        console.log("Python failed with the following traceback:", msg.traceback)
      }
    }
    var comm = HoloViews.comm_manager.get_client_comm(this.plot_id, this.id+'_client', ack_callback);
    return comm
  }
}

HoloViewsWidget.prototype.msg_handler = function(msg) {
  var metadata = msg.metadata;
  if ((metadata.msg_type == "Ready")) {
    if (metadata.content) {
      console.log("Python callback returned following output:", metadata.content);
    }
	return;
  } else if (metadata.msg_type == "Error") {
    console.log("Python failed with the following traceback:", metadata.traceback)
    return
  }
  this.process_msg(msg)
}

HoloViewsWidget.prototype.process_msg = function(msg) {
}

function SelectionWidget(frames, id, slider_ids, keyMap, dim_vals, notFound, load_json, mode, cached, json_path, dynamic, plot_id){
  this.frames = frames;
  this.id = id;
  this.plot_id = plot_id;
  this.slider_ids = slider_ids;
  this.keyMap = keyMap
  this.current_frame = 0;
  this.current_vals = dim_vals;
  this.load_json = load_json;
  this.mode = mode;
  this.notFound = notFound;
  this.cached = cached;
  this.dynamic = dynamic;
  this.cache = {};
  this.json_path = json_path;
  this.init_slider(this.current_vals[0]);
  this.queue = [];
  this.wait = false;
  if (!this.cached || this.dynamic) {
    this.comm = this.init_comms();
  }
}

SelectionWidget.prototype = new HoloViewsWidget;


SelectionWidget.prototype.get_key = function(current_vals) {
  var key = "(";
  for (var i=0; i<this.slider_ids.length; i++)
  {
    var val = this.current_vals[i];
    if (!(typeof val === 'string')) {
      if (val % 1 === 0) { val = val.toFixed(1); }
      else { val = val.toFixed(10); val = val.slice(0, val.length-1);}
    }
    key += "'" + val + "'";
    if(i != this.slider_ids.length-1) { key += ', ';}
    else if(this.slider_ids.length == 1) { key += ',';}
  }
  key += ")";
  return this.keyMap[key];
}

SelectionWidget.prototype.set_frame = function(dim_val, dim_idx){
  this.current_vals[dim_idx] = dim_val;
  var key = this.current_vals;
  if (!this.dynamic) {
    key = this.get_key(key)
  }
  if (this.dynamic || !this.cached) {
    if ((this.time !== undefined) && ((this.wait) && ((this.time + 10000) > Date.now()))) {
      this.queue.push(key);
      return
    }
    this.queue = [];
    this.time = Date.now();
    this.current_frame = key;
    this.wait = true;
    this.dynamic_update(key)
  } else if (key !== undefined) {
    this.update(key)
  }
}


/* Define the ScrubberWidget class */
function ScrubberWidget(frames, num_frames, id, interval, load_json, mode, cached, json_path, dynamic, plot_id){
  this.slider_id = "_anim_slider" + id;
  this.loop_select_id = "_anim_loop_select" + id;
  this.id = id;
  this.plot_id = plot_id;
  this.interval = interval;
  this.current_frame = 0;
  this.direction = 0;
  this.dynamic = dynamic;
  this.timer = null;
  this.load_json = load_json;
  this.mode = mode;
  this.cached = cached;
  this.frames = frames;
  this.cache = {};
  this.length = num_frames;
  this.json_path = json_path;
  document.getElementById(this.slider_id).max = this.length - 1;
  this.init_slider(0);
  this.wait = false;
  this.queue = [];
  if (!this.cached || this.dynamic) {
    this.comm = this.init_comms()
  }
}

ScrubberWidget.prototype = new HoloViewsWidget;

ScrubberWidget.prototype.set_frame = function(frame){
  this.current_frame = frame;
  var widget = document.getElementById(this.slider_id);
  if (widget === null) {
    this.pause_animation();
    return
  }
  widget.value = this.current_frame;
  if (this.dynamic || !this.cached) {
    if ((this.time !== undefined) && ((this.wait) && ((this.time + 10000) > Date.now()))) {
      this.queue.push(frame);
      return
    }
    this.queue = [];
    this.time = Date.now();
    this.wait = true;
    this.dynamic_update(frame)
  } else {
    this.update(frame)
  }
}

ScrubberWidget.prototype.get_loop_state = function(){
  var button_group = document[this.loop_select_id].state;
  for (var i = 0; i < button_group.length; i++) {
    var button = button_group[i];
    if (button.checked) {
      return button.value;
    }
  }
  return undefined;
}


ScrubberWidget.prototype.next_frame = function() {
  this.set_frame(Math.min(this.length - 1, this.current_frame + 1));
}

ScrubberWidget.prototype.previous_frame = function() {
  this.set_frame(Math.max(0, this.current_frame - 1));
}

ScrubberWidget.prototype.first_frame = function() {
  this.set_frame(0);
}

ScrubberWidget.prototype.last_frame = function() {
  this.set_frame(this.length - 1);
}

ScrubberWidget.prototype.slower = function() {
  this.interval /= 0.7;
  if(this.direction > 0){this.play_animation();}
  else if(this.direction < 0){this.reverse_animation();}
}

ScrubberWidget.prototype.faster = function() {
  this.interval *= 0.7;
  if(this.direction > 0){this.play_animation();}
  else if(this.direction < 0){this.reverse_animation();}
}

ScrubberWidget.prototype.anim_step_forward = function() {
  if(this.current_frame < this.length - 1){
    this.next_frame();
  }else{
    var loop_state = this.get_loop_state();
    if(loop_state == "loop"){
      this.first_frame();
    }else if(loop_state == "reflect"){
      this.last_frame();
      this.reverse_animation();
    }else{
      this.pause_animation();
      this.last_frame();
    }
  }
}

ScrubberWidget.prototype.anim_step_reverse = function() {
  if(this.current_frame > 0){
    this.previous_frame();
  } else {
    var loop_state = this.get_loop_state();
    if(loop_state == "loop"){
      this.last_frame();
    }else if(loop_state == "reflect"){
      this.first_frame();
      this.play_animation();
    }else{
      this.pause_animation();
      this.first_frame();
    }
  }
}

ScrubberWidget.prototype.pause_animation = function() {
  this.direction = 0;
  if (this.timer){
    clearInterval(this.timer);
    this.timer = null;
  }
}

ScrubberWidget.prototype.play_animation = function() {
  this.pause_animation();
  this.direction = 1;
  var t = this;
  if (!this.timer) this.timer = setInterval(function(){t.anim_step_forward();}, this.interval);
}

ScrubberWidget.prototype.reverse_animation = function() {
  this.pause_animation();
  this.direction = -1;
  var t = this;
  if (!this.timer) this.timer = setInterval(function(){t.anim_step_reverse();}, this.interval);
}

function extend(destination, source) {
  for (var k in source) {
    if (source.hasOwnProperty(k)) {
      destination[k] = source[k];
    }
  }
  return destination;
}

function update_widget(widget, values) {
  if (widget.hasClass("ui-slider")) {
    widget.slider('option', {
      min: 0,
      max: values.length-1,
      dim_vals: values,
      value: 0,
      dim_labels: values
    })
    widget.slider('option', 'slide').call(widget, event, {value: 0})
  } else {
    widget.empty();
    for (var i=0; i<values.length; i++){
      widget.append($("<option>", {
        value: i,
        text: values[i]
      }))
    };
    widget.data('values', values);
    widget.data('value', 0);
    widget.trigger("change");
  };
}

function init_slider(id, plot_id, dim, values, next_vals, labels, dynamic, step, value, next_dim,
                     dim_idx, delay, jQueryUI_CDN, UNDERSCORE_CDN) {
  // Slider JS Block START
  function loadcssfile(filename){
    var fileref=document.createElement("link")
    fileref.setAttribute("rel", "stylesheet")
    fileref.setAttribute("type", "text/css")
    fileref.setAttribute("href", filename)
    document.getElementsByTagName("head")[0].appendChild(fileref)
  }
  loadcssfile("https://code.jquery.com/ui/1.10.4/themes/smoothness/jquery-ui.css");
  /* Check if jQuery and jQueryUI have been loaded
     otherwise load with require.js */
  var jQuery = window.jQuery,
    // check for old versions of jQuery
    oldjQuery = jQuery && !!jQuery.fn.jquery.match(/^1\.[0-4](\.|$)/),
    jquery_path = '',
    paths = {},
    noConflict;
  var jQueryUI = jQuery.ui;
  // check for jQuery
  if (!jQuery || oldjQuery) {
    // load if it's not available or doesn't meet min standards
    paths.jQuery = jQuery;
    noConflict = !!oldjQuery;
  } else {
    // register the current jQuery
    define('jquery', [], function() { return jQuery; });
  }
  if (!jQueryUI) {
    paths.jQueryUI = jQueryUI_CDN.slice(null, -3);
  } else {
    define('jQueryUI', [], function() { return jQuery.ui; });
  }
  paths.underscore = UNDERSCORE_CDN.slice(null, -3);
  var jquery_require = {
    paths: paths,
    shim: {
      "jQueryUI": {
        exports:"$",
        deps: ['jquery']
      },
      "underscore": {
        exports: '_'
      }
    }
  }
  require.config(jquery_require);
  require(["jQueryUI", "underscore"], function(jUI, _){
    if (noConflict) $.noConflict(true);
    var vals = values;
    if (dynamic && vals.constructor === Array) {
      var default_value = parseFloat(value);
      var min = parseFloat(vals[0]);
      var max = parseFloat(vals[vals.length-1]);
      var wstep = step;
      var wlabels = [default_value];
      var init_label = default_value;
    } else {
      var min = 0;
      if (dynamic) {
        var max = Object.keys(vals).length - 1;
        var init_label = labels[value];
        var default_value = values[value];
      } else {
        var max = vals.length - 1;
        var init_label = labels[value];
        var default_value = value;
      }
      var wstep = 1;
      var wlabels = labels;
    }
    function adjustFontSize(text) {
      var width_ratio = (text.parent().width()/8)/text.val().length;
      var size = Math.min(0.9, Math.max(0.6, width_ratio))+'em';
      text.css('font-size', size);
    }
    var slider = $('#_anim_widget'+id+'_'+dim);
    slider.slider({
      animate: "fast",
      min: min,
      max: max,
      step: wstep,
      value: default_value,
      dim_vals: vals,
      dim_labels: wlabels,
      next_vals: next_vals,
      slide: function(event, ui) {
        var vals = slider.slider("option", "dim_vals");
        var next_vals = slider.slider("option", "next_vals");
        var dlabels = slider.slider("option", "dim_labels");
        if (dynamic) {
          var dim_val = ui.value;
          if (vals.constructor === Array) {
            var label = ui.value;
          } else {
            var label = dlabels[ui.value];
          }
        } else {
          var dim_val = vals[ui.value];
          var label = dlabels[ui.value];
        }
        var text = $('#textInput'+id+'_'+dim);
        text.val(label);
        adjustFontSize(text);
        HoloViews.index[plot_id].set_frame(dim_val, dim_idx);
        if (Object.keys(next_vals).length > 0) {
          var new_vals = next_vals[dim_val];
          var next_widget = $('#_anim_widget'+id+'_'+next_dim);
          update_widget(next_widget, new_vals);
        }
      }
    });
    slider.keypress(function(event) {
      if (event.which == 80 || event.which == 112) {
        var start = slider.slider("option", "value");
        var stop =  slider.slider("option", "max");
        for (var i=start; i<=stop; i++) {
          var delay = i*delay;
          $.proxy(function doSetTimeout(i) { setTimeout($.proxy(function() {
            var val = {value:i};
            slider.slider('value',i);
            slider.slider("option", "slide")(null, val);
          }, slider), delay);}, slider)(i);
        }
      }
      if (event.which == 82 || event.which == 114) {
        var start = slider.slider("option", "value");
        var stop =  slider.slider("option", "min");
        var count = 0;
        for (var i=start; i>=stop; i--) {
          var delay = count*delay;
          count = count + 1;
          $.proxy(function doSetTimeout(i) { setTimeout($.proxy(function() {
            var val = {value:i};
            slider.slider('value',i);
            slider.slider("option", "slide")(null, val);
          }, slider), delay);}, slider)(i);
        }
      }
    });
    var textInput = $('#textInput'+id+'_'+dim)
    textInput.val(init_label);
    adjustFontSize(textInput);
  });
}

function init_dropdown(id, plot_id, dim, vals, value, next_vals, labels, next_dim, dim_idx, dynamic) {
  var widget = $("#_anim_widget"+id+'_'+dim);
  widget.data('values', vals)
  for (var i=0; i<vals.length; i++){
    if (dynamic) {
      var val = vals[i];
    } else {
      var val = i;
    }
    widget.append($("<option>", {
      value: val,
      text: labels[i]
    }));
  };
  widget.data("next_vals", next_vals);
  widget.val(value);
  widget.on('change', function(event, ui) {
    if (dynamic) {
      var dim_val = parseInt(this.value);
    } else {
      var dim_val = $.data(this, 'values')[this.value];
    }
    var next_vals = $.data(this, "next_vals");
    if (Object.keys(next_vals).length > 0) {
      var new_vals = next_vals[dim_val];
      var next_widget = $('#_anim_widget'+id+'_'+next_dim);
      update_widget(next_widget, new_vals);
    }
    var widgets = HoloViews.index[plot_id]
    if (widgets) {
      widgets.set_frame(dim_val, dim_idx);
    }
  });
}


if (window.HoloViews === undefined) {
  window.HoloViews = {}
  window.PyViz = window.HoloViews
} else if (window.PyViz === undefined) {
  window.PyViz = window.HoloViews
}


var _namespace = {
  init_slider: init_slider,
  init_dropdown: init_dropdown,
  comms: {},
  comm_status: {},
  index: {},
  plot_index: {},
  kernels: {},
  receivers: {}
}

for (var k in _namespace) {
  if (!(k in window.HoloViews)) {
    window.HoloViews[k] = _namespace[k];
  }
}

// Define Bokeh specific subclasses
function BokehSelectionWidget() {
  SelectionWidget.apply(this, arguments);
}

function BokehScrubberWidget() {
  ScrubberWidget.apply(this, arguments);
}

// Let them inherit from the baseclasses
BokehSelectionWidget.prototype = Object.create(SelectionWidget.prototype);
BokehScrubberWidget.prototype = Object.create(ScrubberWidget.prototype);

// Define methods to override on widgets
var BokehMethods = {
  update_cache : function(){
    for (var index in this.frames) {
      this.frames[index] = JSON.parse(this.frames[index]);
    }
  },
  update : function(current){
    if (current === undefined) {
      return;
    }
    var data = this.frames[current];
    if (data !== undefined) {
      if (data.root in HoloViews.plot_index) {
        var doc = HoloViews.plot_index[data.root].model.document;
      } else {
        var doc = Bokeh.index[data.root].model.document;
      }
      doc.apply_json_patch(data.content);
    }
  },
  init_comms: function() {
    if (Bokeh.protocol !== undefined) {
      this.receiver = new Bokeh.protocol.Receiver()
    } else {
      this.receiver = null;
    }
    return HoloViewsWidget.prototype.init_comms.call(this);
  },
  process_msg : function(msg) {
    if (this.plot_id in HoloViews.plot_index) {
      var doc = HoloViews.plot_index[this.plot_id].model.document;
    } else {
      var doc = Bokeh.index[this.plot_id].model.document;
    }
    if (this.receiver === null) { return }
    var receiver = this.receiver;
    if (msg.buffers.length > 0) {
      receiver.consume(msg.buffers[0].buffer)
    } else {
      receiver.consume(msg.content.data)
    }
    const comm_msg = receiver.message;
    if ((comm_msg != null) && (doc != null)) {
      doc.apply_json_patch(comm_msg.content, comm_msg.buffers)
    }
  }
}

// Extend Bokeh widgets with backend specific methods
extend(BokehSelectionWidget.prototype, BokehMethods);
extend(BokehScrubberWidget.prototype, BokehMethods);

window.HoloViews.BokehSelectionWidget = BokehSelectionWidget
window.HoloViews.BokehScrubberWidget = BokehScrubberWidget
</script>
<script type="text/javascript">
    function CommManager() {
    }

    CommManager.prototype.register_target = function() {
    }

    CommManager.prototype.get_client_comm = function() {
    }

    window.PyViz.comm_manager = CommManager()
    </script>
  </head>
  <body>
    <div class="hololayout row row-fluid">
  <div class="holoframe" id="display_area081d313bb718490db7b0a0e545063ce1">
    <div id="_anim_img081d313bb718490db7b0a0e545063ce1">
      
      <div id='1358' style='display: table; margin: 0 auto;'>





  <div class="bk-root" id="1c360703-a09e-4f30-acb3-279a51b55767" data-root-id="1358"></div>
</div>
      
    </div>
  </div>
  <div class="holowidgets" id="widget_area081d313bb718490db7b0a0e545063ce1">
    <form class="holoform well" id="form081d313bb718490db7b0a0e545063ce1">
      
      
      <div class="form-group control-group holoformgroup" style=''>
        <label for="textInput081d313bb718490db7b0a0e545063ce1_Time">
          <strong>Time:</strong>
        </label>
        <div class="holowell">
          <div class="hologroup">
            <input type="text" class="holotext form-control input-small"
                   id="textInput081d313bb718490db7b0a0e545063ce1_Time" value="" readonly>
          </div>
          <div class="holoslider"
               id="_anim_widget081d313bb718490db7b0a0e545063ce1_Time"></div>
        </div>
      </div>
      
        
        </form>
    </div>
</div>
<script type="text/javascript">/* Instantiate the BokehSelectionWidget class. */
/* The IDs given should match those used in the template above. */
var widget_ids = new Array(1);


widget_ids[0] = "_anim_widget081d313bb718490db7b0a0e545063ce1_Time";


function create_widget() {
  var frame_data = {"0": "{\"content\":{\"events\":[{\"cols\":[\"end\",\"start\"],\"column_source\":{\"id\":\"1039\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"end\":{\"__ndarray__\":\"AwAAAAkAAAAKAAAACwAAAAwAAAANAAAADwAAABAAAAADAAAACQAAAAoAAAAMAAAADQAAAA4AAAAPAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAADAAAACgAAAAsAAAAMAAAADQAAAA4AAAAPAAAACgAAAAsAAAAMAAAADgAAAAoAAAAMAAAADQAAAA8AAAAMAAAADQAAAA4AAAAPAAAADQAAAA8AAAAQAAAADAAAAA4AAAAPAAAAEAAAAA4AAAAPAAAAEAAAAAYAAAAPAAAAEAAAAAQAAAAGAAAADwAAABAAAAAEAAAABgAAAAcAAAAIAAAADgAAAA8AAAAQAAAABAAAAAYAAAAHAAAACAAAAA0AAAAOAAAAEAAAAAQAAAAFAAAABgAAAAcAAAAIAAAADgAAAA8AAAAEAAAABQAAAAYAAAAPAAAAEAAAABAAAAAFAAAAEAAAAAUAAAA=\",\"dtype\":\"int32\",\"shape\":[89]},\"start\":{\"__ndarray__\":\"EgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAXAAAAFwAAABcAAAAXAAAAFwAAABcAAAAXAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAABQAAAAUAAAAFAAAABQAAAARAAAAEQAAABEAAAARAAAAGwAAABsAAAAbAAAAHAAAABwAAAAcAAAAHAAAAB8AAAAfAAAAHwAAAB8AAAAYAAAAGAAAABgAAAAYAAAAFgAAABYAAAAWAAAAFgAAABYAAAAWAAAAHQAAAB0AAAAdAAAAHQAAAB0AAAAdAAAAHQAAABkAAAAZAAAAGQAAABkAAAAZAAAAGQAAABkAAAAZAAAAFQAAABUAAAAVAAAAFQAAABUAAAACAAAAAgAAABoAAAAaAAAAEwAAABMAAAA=\",\"dtype\":\"int32\",\"shape\":[89]}}},{\"cols\":[\"status\",\"bipartite\",\"infected\",\"degree_centrality\",\"recovered\",\"index\",\"index_hover\",\"node_color\"],\"column_source\":{\"id\":\"1038\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"bipartite\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"degree_centrality\":{\"__ndarray__\":\"55xzzjnnzD+EEEIIIYTAP4QQQgghhLA/xhhjjDHGuD+llFJKKaXEP4QQQgghhMA/xhhjjDHGyD/GGGOMMca4P8YYY4wxxrg/xhhjjDHGuD/GGGOMMcbIP4QQQgghhMA/hBBCCCGE0D+EEEIIIYTQP6WUUkoppdQ/55xzzjnn3D/GGGOMMcbYP4QQQgghhMA/hBBCCCGE0D+EEEIIIYSwP4QQQgghhMA/pZRSSimlxD/GGGOMMcbIP+ecc84558w/hBBCCCGEwD+EEEIIIYTQP4QQQgghhLA/xhhjjDHGuD+EEEIIIYTAP+ecc84558w/hBBCCCGE0D+EEEIIIYTAPw==\",\"dtype\":\"float64\",\"shape\":[32]},\"index\":{\"__ndarray__\":\"AAAAAAEAAAACAAAAAwAAAAQAAAAFAAAABgAAAAcAAAAIAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAARAAAAEgAAABMAAAAUAAAAFQAAABYAAAAXAAAAGAAAABkAAAAaAAAAGwAAABwAAAAdAAAAHgAAAB8AAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"index_hover\":[\"Brenda Rogers\",\"Charlotte McDowd\",\"Dorothy Murchison\",\"E1\",\"E10\",\"E11\",\"E12\",\"E13\",\"E14\",\"E2\",\"E3\",\"E4\",\"E5\",\"E6\",\"E7\",\"E8\",\"E9\",\"Eleanor Nye\",\"Evelyn Jefferson\",\"Flora Price\",\"Frances Anderson\",\"Helen Lloyd\",\"Katherina Rogers\",\"Laura Mandeville\",\"Myra Liddel\",\"Nora Fayette\",\"Olivia Carleton\",\"Pearl Oglethorpe\",\"Ruth DeSand\",\"Sylvia Avondale\",\"Theresa Anderson\",\"Verne Sanderson\"],\"infected\":{\"__ndarray__\":\"AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAQAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"node_color\":[\"R\",\"S\",\"S\",\"I\",\"S\",\"S\",\"S\",\"R\",\"S\",\"S\",\"R\",\"R\",\"I\",\"R\",\"I\",\"I\",\"R\",\"R\",\"R\",\"S\",\"R\",\"S\",\"S\",\"I\",\"S\",\"I\",\"S\",\"S\",\"S\",\"S\",\"R\",\"R\"],\"recovered\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"status\":[\"R\",\"S\",\"S\",\"I\",\"S\",\"S\",\"S\",\"R\",\"S\",\"S\",\"R\",\"R\",\"I\",\"R\",\"I\",\"I\",\"R\",\"R\",\"R\",\"S\",\"R\",\"S\",\"S\",\"I\",\"S\",\"I\",\"S\",\"S\",\"S\",\"S\",\"R\",\"R\"]}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1106\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"BgAAAAcAAAACAAAAAAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1122\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"CwAAABEAAAAYAAAAGgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1139\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"DwAAAAgAAAAGAAAABgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"attr\":\"location\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1244\",\"type\":\"Span\"},\"new\":1},{\"attr\":\"text\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1357\",\"type\":\"Div\"},\"new\":\"<span style=\\\"color:black;font-family:Arial;font-style:bold;font-weight:bold;font-size:12pt\\\">Time: 1<\/span>\"},{\"cols\":[\"end\",\"start\"],\"column_source\":{\"id\":\"1039\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"end\":{\"__ndarray__\":\"AwAAAAkAAAAKAAAACwAAAAwAAAANAAAADwAAABAAAAADAAAACQAAAAoAAAAMAAAADQAAAA4AAAAPAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAADAAAACgAAAAsAAAAMAAAADQAAAA4AAAAPAAAACgAAAAsAAAAMAAAADgAAAAoAAAAMAAAADQAAAA8AAAAMAAAADQAAAA4AAAAPAAAADQAAAA8AAAAQAAAADAAAAA4AAAAPAAAAEAAAAA4AAAAPAAAAEAAAAAYAAAAPAAAAEAAAAAQAAAAGAAAADwAAABAAAAAEAAAABgAAAAcAAAAIAAAADgAAAA8AAAAQAAAABAAAAAYAAAAHAAAACAAAAA0AAAAOAAAAEAAAAAQAAAAFAAAABgAAAAcAAAAIAAAADgAAAA8AAAAEAAAABQAAAAYAAAAPAAAAEAAAABAAAAAFAAAAEAAAAAUAAAA=\",\"dtype\":\"int32\",\"shape\":[89]},\"start\":{\"__ndarray__\":\"EgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAXAAAAFwAAABcAAAAXAAAAFwAAABcAAAAXAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAABQAAAAUAAAAFAAAABQAAAARAAAAEQAAABEAAAARAAAAGwAAABsAAAAbAAAAHAAAABwAAAAcAAAAHAAAAB8AAAAfAAAAHwAAAB8AAAAYAAAAGAAAABgAAAAYAAAAFgAAABYAAAAWAAAAFgAAABYAAAAWAAAAHQAAAB0AAAAdAAAAHQAAAB0AAAAdAAAAHQAAABkAAAAZAAAAGQAAABkAAAAZAAAAGQAAABkAAAAZAAAAFQAAABUAAAAVAAAAFQAAABUAAAACAAAAAgAAABoAAAAaAAAAEwAAABMAAAA=\",\"dtype\":\"int32\",\"shape\":[89]}}},{\"cols\":[\"status\",\"bipartite\",\"infected\",\"degree_centrality\",\"recovered\",\"index\",\"index_hover\",\"node_color\"],\"column_source\":{\"id\":\"1038\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"bipartite\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"degree_centrality\":{\"__ndarray__\":\"55xzzjnnzD+EEEIIIYTAP4QQQgghhLA/xhhjjDHGuD+llFJKKaXEP4QQQgghhMA/xhhjjDHGyD/GGGOMMca4P8YYY4wxxrg/xhhjjDHGuD/GGGOMMcbIP4QQQgghhMA/hBBCCCGE0D+EEEIIIYTQP6WUUkoppdQ/55xzzjnn3D/GGGOMMcbYP4QQQgghhMA/hBBCCCGE0D+EEEIIIYSwP4QQQgghhMA/pZRSSimlxD/GGGOMMcbIP+ecc84558w/hBBCCCGEwD+EEEIIIYTQP4QQQgghhLA/xhhjjDHGuD+EEEIIIYTAP+ecc84558w/hBBCCCGE0D+EEEIIIYTAPw==\",\"dtype\":\"float64\",\"shape\":[32]},\"index\":{\"__ndarray__\":\"AAAAAAEAAAACAAAAAwAAAAQAAAAFAAAABgAAAAcAAAAIAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAARAAAAEgAAABMAAAAUAAAAFQAAABYAAAAXAAAAGAAAABkAAAAaAAAAGwAAABwAAAAdAAAAHgAAAB8AAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"index_hover\":[\"Brenda Rogers\",\"Charlotte McDowd\",\"Dorothy Murchison\",\"E1\",\"E10\",\"E11\",\"E12\",\"E13\",\"E14\",\"E2\",\"E3\",\"E4\",\"E5\",\"E6\",\"E7\",\"E8\",\"E9\",\"Eleanor Nye\",\"Evelyn Jefferson\",\"Flora Price\",\"Frances Anderson\",\"Helen Lloyd\",\"Katherina Rogers\",\"Laura Mandeville\",\"Myra Liddel\",\"Nora Fayette\",\"Olivia Carleton\",\"Pearl Oglethorpe\",\"Ruth DeSand\",\"Sylvia Avondale\",\"Theresa Anderson\",\"Verne Sanderson\"],\"infected\":{\"__ndarray__\":\"AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAQAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"node_color\":[\"R\",\"S\",\"S\",\"I\",\"S\",\"S\",\"S\",\"R\",\"S\",\"S\",\"R\",\"R\",\"I\",\"R\",\"I\",\"I\",\"R\",\"R\",\"R\",\"S\",\"R\",\"S\",\"S\",\"I\",\"S\",\"I\",\"S\",\"S\",\"S\",\"S\",\"R\",\"R\"],\"recovered\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"status\":[\"R\",\"S\",\"S\",\"I\",\"S\",\"S\",\"S\",\"R\",\"S\",\"S\",\"R\",\"R\",\"I\",\"R\",\"I\",\"I\",\"R\",\"R\",\"R\",\"S\",\"R\",\"S\",\"S\",\"I\",\"S\",\"I\",\"S\",\"S\",\"S\",\"S\",\"R\",\"R\"]}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1106\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"BgAAAAcAAAACAAAAAAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1122\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"CwAAABEAAAAYAAAAGgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1139\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"DwAAAAgAAAAGAAAABgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}}],\"references\":[]},\"root\":\"1358\"}", "1": "{\"content\":{\"events\":[{\"cols\":[\"end\",\"start\"],\"column_source\":{\"id\":\"1039\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"end\":{\"__ndarray__\":\"AwAAAAkAAAAKAAAACwAAAAwAAAANAAAADwAAABAAAAADAAAACQAAAAoAAAAMAAAADQAAAA4AAAAPAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAADAAAACgAAAAsAAAAMAAAADQAAAA4AAAAPAAAACgAAAAsAAAAMAAAADgAAAAoAAAAMAAAADQAAAA8AAAAMAAAADQAAAA4AAAAPAAAADQAAAA8AAAAQAAAADAAAAA4AAAAPAAAAEAAAAA4AAAAPAAAAEAAAAAYAAAAPAAAAEAAAAAQAAAAGAAAADwAAABAAAAAEAAAABgAAAAcAAAAIAAAADgAAAA8AAAAQAAAABAAAAAYAAAAHAAAACAAAAA0AAAAOAAAAEAAAAAQAAAAFAAAABgAAAAcAAAAIAAAADgAAAA8AAAAEAAAABQAAAAYAAAAPAAAAEAAAABAAAAAFAAAAEAAAAAUAAAA=\",\"dtype\":\"int32\",\"shape\":[89]},\"start\":{\"__ndarray__\":\"EgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAXAAAAFwAAABcAAAAXAAAAFwAAABcAAAAXAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAABQAAAAUAAAAFAAAABQAAAARAAAAEQAAABEAAAARAAAAGwAAABsAAAAbAAAAHAAAABwAAAAcAAAAHAAAAB8AAAAfAAAAHwAAAB8AAAAYAAAAGAAAABgAAAAYAAAAFgAAABYAAAAWAAAAFgAAABYAAAAWAAAAHQAAAB0AAAAdAAAAHQAAAB0AAAAdAAAAHQAAABkAAAAZAAAAGQAAABkAAAAZAAAAGQAAABkAAAAZAAAAFQAAABUAAAAVAAAAFQAAABUAAAACAAAAAgAAABoAAAAaAAAAEwAAABMAAAA=\",\"dtype\":\"int32\",\"shape\":[89]}}},{\"cols\":[\"status\",\"bipartite\",\"infected\",\"degree_centrality\",\"recovered\",\"index\",\"index_hover\",\"node_color\"],\"column_source\":{\"id\":\"1038\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"bipartite\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"degree_centrality\":{\"__ndarray__\":\"55xzzjnnzD+EEEIIIYTAP4QQQgghhLA/xhhjjDHGuD+llFJKKaXEP4QQQgghhMA/xhhjjDHGyD/GGGOMMca4P8YYY4wxxrg/xhhjjDHGuD/GGGOMMcbIP4QQQgghhMA/hBBCCCGE0D+EEEIIIYTQP6WUUkoppdQ/55xzzjnn3D/GGGOMMcbYP4QQQgghhMA/hBBCCCGE0D+EEEIIIYSwP4QQQgghhMA/pZRSSimlxD/GGGOMMcbIP+ecc84558w/hBBCCCGEwD+EEEIIIYTQP4QQQgghhLA/xhhjjDHGuD+EEEIIIYTAP+ecc84558w/hBBCCCGE0D+EEEIIIYTAPw==\",\"dtype\":\"float64\",\"shape\":[32]},\"index\":{\"__ndarray__\":\"AAAAAAEAAAACAAAAAwAAAAQAAAAFAAAABgAAAAcAAAAIAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAARAAAAEgAAABMAAAAUAAAAFQAAABYAAAAXAAAAGAAAABkAAAAaAAAAGwAAABwAAAAdAAAAHgAAAB8AAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"index_hover\":[\"Brenda Rogers\",\"Charlotte McDowd\",\"Dorothy Murchison\",\"E1\",\"E10\",\"E11\",\"E12\",\"E13\",\"E14\",\"E2\",\"E3\",\"E4\",\"E5\",\"E6\",\"E7\",\"E8\",\"E9\",\"Eleanor Nye\",\"Evelyn Jefferson\",\"Flora Price\",\"Frances Anderson\",\"Helen Lloyd\",\"Katherina Rogers\",\"Laura Mandeville\",\"Myra Liddel\",\"Nora Fayette\",\"Olivia Carleton\",\"Pearl Oglethorpe\",\"Ruth DeSand\",\"Sylvia Avondale\",\"Theresa Anderson\",\"Verne Sanderson\"],\"infected\":{\"__ndarray__\":\"AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAQAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"node_color\":[\"R\",\"I\",\"I\",\"R\",\"S\",\"S\",\"I\",\"R\",\"I\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"I\",\"I\",\"R\",\"S\",\"R\",\"S\",\"S\",\"S\",\"I\",\"R\",\"R\"],\"recovered\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"status\":[\"R\",\"I\",\"I\",\"R\",\"S\",\"S\",\"I\",\"R\",\"I\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"I\",\"I\",\"R\",\"S\",\"R\",\"S\",\"S\",\"S\",\"I\",\"R\",\"R\"]}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1106\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"BgAAAAcAAAACAAAAAAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1122\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"CwAAABEAAAAYAAAAGgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1139\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"DwAAAAgAAAAGAAAABgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"attr\":\"location\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1244\",\"type\":\"Span\"},\"new\":2},{\"attr\":\"text\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1357\",\"type\":\"Div\"},\"new\":\"<span style=\\\"color:black;font-family:Arial;font-style:bold;font-weight:bold;font-size:12pt\\\">Time: 2<\/span>\"}],\"references\":[]},\"root\":\"1358\"}", "2": "{\"content\":{\"events\":[{\"cols\":[\"end\",\"start\"],\"column_source\":{\"id\":\"1039\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"end\":{\"__ndarray__\":\"AwAAAAkAAAAKAAAACwAAAAwAAAANAAAADwAAABAAAAADAAAACQAAAAoAAAAMAAAADQAAAA4AAAAPAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAADAAAACgAAAAsAAAAMAAAADQAAAA4AAAAPAAAACgAAAAsAAAAMAAAADgAAAAoAAAAMAAAADQAAAA8AAAAMAAAADQAAAA4AAAAPAAAADQAAAA8AAAAQAAAADAAAAA4AAAAPAAAAEAAAAA4AAAAPAAAAEAAAAAYAAAAPAAAAEAAAAAQAAAAGAAAADwAAABAAAAAEAAAABgAAAAcAAAAIAAAADgAAAA8AAAAQAAAABAAAAAYAAAAHAAAACAAAAA0AAAAOAAAAEAAAAAQAAAAFAAAABgAAAAcAAAAIAAAADgAAAA8AAAAEAAAABQAAAAYAAAAPAAAAEAAAABAAAAAFAAAAEAAAAAUAAAA=\",\"dtype\":\"int32\",\"shape\":[89]},\"start\":{\"__ndarray__\":\"EgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAXAAAAFwAAABcAAAAXAAAAFwAAABcAAAAXAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAABQAAAAUAAAAFAAAABQAAAARAAAAEQAAABEAAAARAAAAGwAAABsAAAAbAAAAHAAAABwAAAAcAAAAHAAAAB8AAAAfAAAAHwAAAB8AAAAYAAAAGAAAABgAAAAYAAAAFgAAABYAAAAWAAAAFgAAABYAAAAWAAAAHQAAAB0AAAAdAAAAHQAAAB0AAAAdAAAAHQAAABkAAAAZAAAAGQAAABkAAAAZAAAAGQAAABkAAAAZAAAAFQAAABUAAAAVAAAAFQAAABUAAAACAAAAAgAAABoAAAAaAAAAEwAAABMAAAA=\",\"dtype\":\"int32\",\"shape\":[89]}}},{\"cols\":[\"status\",\"bipartite\",\"infected\",\"degree_centrality\",\"recovered\",\"index\",\"index_hover\",\"node_color\"],\"column_source\":{\"id\":\"1038\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"bipartite\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"degree_centrality\":{\"__ndarray__\":\"55xzzjnnzD+EEEIIIYTAP4QQQgghhLA/xhhjjDHGuD+llFJKKaXEP4QQQgghhMA/xhhjjDHGyD/GGGOMMca4P8YYY4wxxrg/xhhjjDHGuD/GGGOMMcbIP4QQQgghhMA/hBBCCCGE0D+EEEIIIYTQP6WUUkoppdQ/55xzzjnn3D/GGGOMMcbYP4QQQgghhMA/hBBCCCGE0D+EEEIIIYSwP4QQQgghhMA/pZRSSimlxD/GGGOMMcbIP+ecc84558w/hBBCCCGEwD+EEEIIIYTQP4QQQgghhLA/xhhjjDHGuD+EEEIIIYTAP+ecc84558w/hBBCCCGE0D+EEEIIIYTAPw==\",\"dtype\":\"float64\",\"shape\":[32]},\"index\":{\"__ndarray__\":\"AAAAAAEAAAACAAAAAwAAAAQAAAAFAAAABgAAAAcAAAAIAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAARAAAAEgAAABMAAAAUAAAAFQAAABYAAAAXAAAAGAAAABkAAAAaAAAAGwAAABwAAAAdAAAAHgAAAB8AAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"index_hover\":[\"Brenda Rogers\",\"Charlotte McDowd\",\"Dorothy Murchison\",\"E1\",\"E10\",\"E11\",\"E12\",\"E13\",\"E14\",\"E2\",\"E3\",\"E4\",\"E5\",\"E6\",\"E7\",\"E8\",\"E9\",\"Eleanor Nye\",\"Evelyn Jefferson\",\"Flora Price\",\"Frances Anderson\",\"Helen Lloyd\",\"Katherina Rogers\",\"Laura Mandeville\",\"Myra Liddel\",\"Nora Fayette\",\"Olivia Carleton\",\"Pearl Oglethorpe\",\"Ruth DeSand\",\"Sylvia Avondale\",\"Theresa Anderson\",\"Verne Sanderson\"],\"infected\":{\"__ndarray__\":\"AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAQAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"node_color\":[\"R\",\"R\",\"R\",\"R\",\"I\",\"S\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"I\",\"R\",\"S\",\"S\",\"S\",\"R\",\"R\",\"R\"],\"recovered\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"status\":[\"R\",\"R\",\"R\",\"R\",\"I\",\"S\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"I\",\"R\",\"S\",\"S\",\"S\",\"R\",\"R\",\"R\"]}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1106\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"BgAAAAcAAAACAAAAAAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1122\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"CwAAABEAAAAYAAAAGgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1139\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"DwAAAAgAAAAGAAAABgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"attr\":\"location\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1244\",\"type\":\"Span\"},\"new\":3},{\"attr\":\"text\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1357\",\"type\":\"Div\"},\"new\":\"<span style=\\\"color:black;font-family:Arial;font-style:bold;font-weight:bold;font-size:12pt\\\">Time: 3<\/span>\"}],\"references\":[]},\"root\":\"1358\"}", "3": "{\"content\":{\"events\":[{\"cols\":[\"end\",\"start\"],\"column_source\":{\"id\":\"1039\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"end\":{\"__ndarray__\":\"AwAAAAkAAAAKAAAACwAAAAwAAAANAAAADwAAABAAAAADAAAACQAAAAoAAAAMAAAADQAAAA4AAAAPAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAADAAAACgAAAAsAAAAMAAAADQAAAA4AAAAPAAAACgAAAAsAAAAMAAAADgAAAAoAAAAMAAAADQAAAA8AAAAMAAAADQAAAA4AAAAPAAAADQAAAA8AAAAQAAAADAAAAA4AAAAPAAAAEAAAAA4AAAAPAAAAEAAAAAYAAAAPAAAAEAAAAAQAAAAGAAAADwAAABAAAAAEAAAABgAAAAcAAAAIAAAADgAAAA8AAAAQAAAABAAAAAYAAAAHAAAACAAAAA0AAAAOAAAAEAAAAAQAAAAFAAAABgAAAAcAAAAIAAAADgAAAA8AAAAEAAAABQAAAAYAAAAPAAAAEAAAABAAAAAFAAAAEAAAAAUAAAA=\",\"dtype\":\"int32\",\"shape\":[89]},\"start\":{\"__ndarray__\":\"EgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAXAAAAFwAAABcAAAAXAAAAFwAAABcAAAAXAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAABQAAAAUAAAAFAAAABQAAAARAAAAEQAAABEAAAARAAAAGwAAABsAAAAbAAAAHAAAABwAAAAcAAAAHAAAAB8AAAAfAAAAHwAAAB8AAAAYAAAAGAAAABgAAAAYAAAAFgAAABYAAAAWAAAAFgAAABYAAAAWAAAAHQAAAB0AAAAdAAAAHQAAAB0AAAAdAAAAHQAAABkAAAAZAAAAGQAAABkAAAAZAAAAGQAAABkAAAAZAAAAFQAAABUAAAAVAAAAFQAAABUAAAACAAAAAgAAABoAAAAaAAAAEwAAABMAAAA=\",\"dtype\":\"int32\",\"shape\":[89]}}},{\"cols\":[\"status\",\"bipartite\",\"infected\",\"degree_centrality\",\"recovered\",\"index\",\"index_hover\",\"node_color\"],\"column_source\":{\"id\":\"1038\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"bipartite\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"degree_centrality\":{\"__ndarray__\":\"55xzzjnnzD+EEEIIIYTAP4QQQgghhLA/xhhjjDHGuD+llFJKKaXEP4QQQgghhMA/xhhjjDHGyD/GGGOMMca4P8YYY4wxxrg/xhhjjDHGuD/GGGOMMcbIP4QQQgghhMA/hBBCCCGE0D+EEEIIIYTQP6WUUkoppdQ/55xzzjnn3D/GGGOMMcbYP4QQQgghhMA/hBBCCCGE0D+EEEIIIYSwP4QQQgghhMA/pZRSSimlxD/GGGOMMcbIP+ecc84558w/hBBCCCGEwD+EEEIIIYTQP4QQQgghhLA/xhhjjDHGuD+EEEIIIYTAP+ecc84558w/hBBCCCGE0D+EEEIIIYTAPw==\",\"dtype\":\"float64\",\"shape\":[32]},\"index\":{\"__ndarray__\":\"AAAAAAEAAAACAAAAAwAAAAQAAAAFAAAABgAAAAcAAAAIAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAARAAAAEgAAABMAAAAUAAAAFQAAABYAAAAXAAAAGAAAABkAAAAaAAAAGwAAABwAAAAdAAAAHgAAAB8AAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"index_hover\":[\"Brenda Rogers\",\"Charlotte McDowd\",\"Dorothy Murchison\",\"E1\",\"E10\",\"E11\",\"E12\",\"E13\",\"E14\",\"E2\",\"E3\",\"E4\",\"E5\",\"E6\",\"E7\",\"E8\",\"E9\",\"Eleanor Nye\",\"Evelyn Jefferson\",\"Flora Price\",\"Frances Anderson\",\"Helen Lloyd\",\"Katherina Rogers\",\"Laura Mandeville\",\"Myra Liddel\",\"Nora Fayette\",\"Olivia Carleton\",\"Pearl Oglethorpe\",\"Ruth DeSand\",\"Sylvia Avondale\",\"Theresa Anderson\",\"Verne Sanderson\"],\"infected\":{\"__ndarray__\":\"AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAQAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"node_color\":[\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"S\",\"S\",\"R\",\"R\",\"R\"],\"recovered\":{\"__ndarray__\":\"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\"dtype\":\"int32\",\"shape\":[32]},\"status\":[\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"R\",\"R\",\"R\",\"R\",\"R\",\"R\",\"S\",\"S\",\"S\",\"R\",\"R\",\"R\"]}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1106\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"BgAAAAcAAAACAAAAAAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1122\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"CwAAABEAAAAYAAAAGgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"cols\":[\"Time\",\"Value\"],\"column_source\":{\"id\":\"1139\",\"type\":\"ColumnDataSource\"},\"kind\":\"ColumnDataChanged\",\"new\":{\"Time\":{\"__ndarray__\":\"AQAAAAIAAAADAAAABAAAAA==\",\"dtype\":\"int32\",\"shape\":[4]},\"Value\":{\"__ndarray__\":\"DwAAAAgAAAAGAAAABgAAAA==\",\"dtype\":\"int32\",\"shape\":[4]}}},{\"attr\":\"location\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1244\",\"type\":\"Span\"},\"new\":4},{\"attr\":\"text\",\"kind\":\"ModelChanged\",\"model\":{\"id\":\"1357\",\"type\":\"Div\"},\"new\":\"<span style=\\\"color:black;font-family:Arial;font-style:bold;font-weight:bold;font-size:12pt\\\">Time: 4<\/span>\"}],\"references\":[]},\"root\":\"1358\"}"};
  var dim_vals = ['1.0'];
  var keyMap = {"('1.0',)": 0, "('2.0',)": 1, "('3.0',)": 2, "('4.0',)": 3};
  var notFound = "<h2 style='vertical-align: middle>No frame at selected dimension value.<h2>";

  var anim = new HoloViews.BokehSelectionWidget(frame_data, "081d313bb718490db7b0a0e545063ce1", widget_ids,
  keyMap, dim_vals, notFound, false, "default",
  true, "", false, "1358");

  HoloViews.index['1358'] = anim;
}




HoloViews.init_slider('081d313bb718490db7b0a0e545063ce1', '1358', 'Time', ['1.0', '2.0', '3.0', '4.0'], {}, ['1', '2', '3', '4'], false, 1, 0, 'None', 0, 200, 'https://code.jquery.com/ui/1.10.4/jquery-ui.min.js', 'https://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.8.3/underscore-min.js')




create_widget();
(function(root) {
  function embed_document(root) {
    
  var docs_json = {"0149d744-b9c8-44b6-b64e-9b1861cb3af6":{"roots":{"references":[{"attributes":{"children":[{"id":"1357","type":"Div"},{"id":"1356","type":"Column"}]},"id":"1358","type":"Column"},{"attributes":{"children":[[{"id":"1004","subtype":"Figure","type":"Plot"},0,0],[{"id":"1072","subtype":"Figure","type":"Plot"},0,1]]},"id":"1353","type":"GridBox"},{"attributes":{"source":{"id":"1038","type":"ColumnDataSource"}},"id":"1047","type":"CDSView"},{"attributes":{"fill_color":{"field":"node_color","transform":{"id":"1037","type":"CategoricalColorMapper"}},"line_color":{"value":"black"},"size":{"units":"screen","value":15}},"id":"1043","type":"Circle"},{"attributes":{"source":{"id":"1039","type":"ColumnDataSource"}},"id":"1052","type":"CDSView"},{"attributes":{"fill_color":{"value":"limegreen"},"size":{"units":"screen","value":15}},"id":"1044","type":"Circle"},{"attributes":{"active_drag":"auto","active_inspect":"auto","active_multi":null,"active_scroll":"auto","active_tap":"auto","tools":[{"id":"1003","type":"HoverTool"},{"id":"1023","type":"SaveTool"},{"id":"1024","type":"PanTool"},{"id":"1025","type":"WheelZoomTool"},{"id":"1026","type":"BoxZoomTool"},{"id":"1027","type":"ResetTool"},{"id":"1028","type":"TapTool"}]},"id":"1029","type":"Toolbar"},{"attributes":{"callback":null,"data":{"bipartite":{"__ndarray__":"AAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAABAAAAAQAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=","dtype":"int32","shape":[32]},"degree_centrality":{"__ndarray__":"55xzzjnnzD+EEEIIIYTAP4QQQgghhLA/xhhjjDHGuD+llFJKKaXEP4QQQgghhMA/xhhjjDHGyD/GGGOMMca4P8YYY4wxxrg/xhhjjDHGuD/GGGOMMcbIP4QQQgghhMA/hBBCCCGE0D+EEEIIIYTQP6WUUkoppdQ/55xzzjnn3D/GGGOMMcbYP4QQQgghhMA/hBBCCCGE0D+EEEIIIYSwP4QQQgghhMA/pZRSSimlxD/GGGOMMcbIP+ecc84558w/hBBCCCGEwD+EEEIIIYTQP4QQQgghhLA/xhhjjDHGuD+EEEIIIYTAP+ecc84558w/hBBCCCGE0D+EEEIIIYTAPw==","dtype":"float64","shape":[32]},"index":{"__ndarray__":"AAAAAAEAAAACAAAAAwAAAAQAAAAFAAAABgAAAAcAAAAIAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAARAAAAEgAAABMAAAAUAAAAFQAAABYAAAAXAAAAGAAAABkAAAAaAAAAGwAAABwAAAAdAAAAHgAAAB8AAAA=","dtype":"int32","shape":[32]},"index_hover":["Brenda Rogers","Charlotte McDowd","Dorothy Murchison","E1","E10","E11","E12","E13","E14","E2","E3","E4","E5","E6","E7","E8","E9","Eleanor Nye","Evelyn Jefferson","Flora Price","Frances Anderson","Helen Lloyd","Katherina Rogers","Laura Mandeville","Myra Liddel","Nora Fayette","Olivia Carleton","Pearl Oglethorpe","Ruth DeSand","Sylvia Avondale","Theresa Anderson","Verne Sanderson"],"infected":{"__ndarray__":"AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAQAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAA=","dtype":"int32","shape":[32]},"node_color":["R","S","S","I","S","S","S","R","S","S","R","R","I","R","I","I","R","R","R","S","R","S","S","I","S","I","S","S","S","S","R","R"],"recovered":{"__ndarray__":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAEAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=","dtype":"int32","shape":[32]},"status":["R","S","S","I","S","S","S","R","S","S","R","R","I","R","I","I","R","R","R","S","R","S","S","I","S","I","S","S","S","S","R","R"]},"selected":{"id":"1337","type":"Selection"},"selection_policy":{"id":"1338","type":"UnionRenderers"}},"id":"1038","type":"ColumnDataSource"},{"attributes":{"callback":null,"end":0.9828835659899119,"reset_end":0.9828835659899119,"reset_start":-1.095741158434749,"start":-1.095741158434749,"tags":[[["x","x",null]]]},"id":"1001","type":"Range1d"},{"attributes":{},"id":"1024","type":"PanTool"},{"attributes":{},"id":"1027","type":"ResetTool"},{"attributes":{"fill_color":{"field":"node_color","transform":{"id":"1037","type":"CategoricalColorMapper"}},"size":{"units":"screen","value":15}},"id":"1041","type":"Circle"},{"attributes":{"overlay":{"id":"1334","type":"BoxAnnotation"}},"id":"1026","type":"BoxZoomTool"},{"attributes":{},"id":"1064","type":"NodesAndLinkedEdges"},{"attributes":{"callback":null,"end":1.1734900243470692,"reset_end":1.1734900243470692,"reset_start":-1.1975900022133699,"start":-1.1975900022133699,"tags":[[["y","y",null]]]},"id":"1002","type":"Range1d"},{"attributes":{},"id":"1067","type":"BasicTickFormatter"},{"attributes":{},"id":"1025","type":"WheelZoomTool"},{"attributes":{},"id":"1095","type":"ResetTool"},{"attributes":{"edge_renderer":{"id":"1051","type":"GlyphRenderer"},"inspection_policy":{"id":"1064","type":"NodesAndLinkedEdges"},"layout_provider":{"id":"1040","type":"StaticLayoutProvider"},"node_renderer":{"id":"1046","type":"GlyphRenderer"},"selection_policy":{"id":"1062","type":"NodesAndLinkedEdges"}},"id":"1053","type":"GraphRenderer"},{"attributes":{"fill_color":{"field":"node_color","transform":{"id":"1037","type":"CategoricalColorMapper"}},"line_color":{"value":"black"},"size":{"units":"screen","value":15}},"id":"1045","type":"Circle"},{"attributes":{"callback":null,"data":{"end":{"__ndarray__":"AwAAAAkAAAAKAAAACwAAAAwAAAANAAAADwAAABAAAAADAAAACQAAAAoAAAAMAAAADQAAAA4AAAAPAAAACQAAAAoAAAALAAAADAAAAA0AAAAOAAAADwAAABAAAAADAAAACgAAAAsAAAAMAAAADQAAAA4AAAAPAAAACgAAAAsAAAAMAAAADgAAAAoAAAAMAAAADQAAAA8AAAAMAAAADQAAAA4AAAAPAAAADQAAAA8AAAAQAAAADAAAAA4AAAAPAAAAEAAAAA4AAAAPAAAAEAAAAAYAAAAPAAAAEAAAAAQAAAAGAAAADwAAABAAAAAEAAAABgAAAAcAAAAIAAAADgAAAA8AAAAQAAAABAAAAAYAAAAHAAAACAAAAA0AAAAOAAAAEAAAAAQAAAAFAAAABgAAAAcAAAAIAAAADgAAAA8AAAAEAAAABQAAAAYAAAAPAAAAEAAAABAAAAAFAAAAEAAAAAUAAAA=","dtype":"int32","shape":[89]},"start":{"__ndarray__":"EgAAABIAAAASAAAAEgAAABIAAAASAAAAEgAAABIAAAAXAAAAFwAAABcAAAAXAAAAFwAAABcAAAAXAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAeAAAAHgAAAB4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAEAAAABAAAAAQAAABQAAAAUAAAAFAAAABQAAAARAAAAEQAAABEAAAARAAAAGwAAABsAAAAbAAAAHAAAABwAAAAcAAAAHAAAAB8AAAAfAAAAHwAAAB8AAAAYAAAAGAAAABgAAAAYAAAAFgAAABYAAAAWAAAAFgAAABYAAAAWAAAAHQAAAB0AAAAdAAAAHQAAAB0AAAAdAAAAHQAAABkAAAAZAAAAGQAAABkAAAAZAAAAGQAAABkAAAAZAAAAFQAAABUAAAAVAAAAFQAAABUAAAACAAAAAgAAABoAAAAaAAAAEwAAABMAAAA=","dtype":"int32","shape":[89]}},"selected":{"id":"1335","type":"Selection"},"selection_policy":{"id":"1336","type":"UnionRenderers"}},"id":"1039","type":"ColumnDataSource"},{"attributes":{"graph_layout":{"0":[0.1845934498212011,0.590185647282396],"1":[0.3367980489762728,0.829256368262244],"10":[0.5918779834320969,0.6498325953851742],"11":[0.5230166049404783,0.8661539312837878],"12":[0.5672609941976295,0.41691299057274095],"13":[0.23388613522460147,0.13334783231517114],"14":[-0.01613123316747828,0.22803148844724766],"15":[0.08067521740439328,-0.02376668341433057],"16":[-0.01520073941854605,-0.33388494276359115],"17":[0.5061734203415016,0.05827795624834337],"18":[0.31117233102893127,0.42143300704864173],"19":[-0.38427539380441944,-1.0],"2":[0.31058465483603853,-0.6322175426442932],"20":[0.7023428000268783,0.1983419738935121],"21":[-0.5308055578751553,-0.04020865798463041],"22":[-0.5150090626480511,-0.4050456054006245],"23":[0.3384983689849743,0.550858444265728],"24":[-0.32830967979242603,-0.4289221016096266],"25":[-0.4457603539336833,-0.19965172576794224],"26":[-0.19019175884043424,-0.9848259996994604],"27":[0.4050000802223267,-0.3536091318522558],"28":[0.3554763185999592,-0.08562681002642518],"29":[-0.38472228700167566,-0.25382741915467977],"3":[0.14312668311630555,0.9759000221336993],"30":[0.4035790876788101,0.29080086884814044],"31":[-0.3101566842088769,0.07267433084073567],"4":[-0.8080844441309442,-0.1904939637044893],"5":[-0.6444960526500165,-0.7403687384611831],"6":[-0.6729380862234318,-0.20870218768565904],"7":[-0.9225224313993605,-0.40447898199192617],"8":[-0.6351232526924212,-0.6117724498613075],"9":[0.8096648389545235,0.6153954851948626]}},"id":"1040","type":"StaticLayoutProvider"},{"attributes":{},"id":"1009","type":"LinearScale"},{"attributes":{"text":"Network","text_color":{"value":"black"},"text_font_size":{"value":"12pt"}},"id":"1005","type":"Title"},{"attributes":{},"id":"1062","type":"NodesAndLinkedEdges"},{"attributes":{"callback":null,"data":{"Time":{"__ndarray__":"AQAAAAIAAAADAAAABAAAAA==","dtype":"int32","shape":[4]},"Value":{"__ndarray__":"BgAAAAcAAAACAAAAAAAAAA==","dtype":"int32","shape":[4]}},"selected":{"id":"1107","type":"Selection"},"selection_policy":{"id":"1136","type":"UnionRenderers"}},"id":"1106","type":"ColumnDataSource"},{"attributes":{"callback":null},"id":"1028","type":"TapTool"},{"attributes":{"grid_line_color":{"value":null},"ticker":{"id":"1014","type":"BasicTicker"}},"id":"1017","type":"Grid"},{"attributes":{},"id":"1011","type":"LinearScale"},{"attributes":{"data_source":{"id":"1038","type":"ColumnDataSource"},"glyph":{"id":"1041","type":"Circle"},"hover_glyph":{"id":"1044","type":"Circle"},"muted_glyph":{"id":"1045","type":"Circle"},"nonselection_glyph":{"id":"1042","type":"Circle"},"selection_glyph":{"id":"1043","type":"Circle"},"view":{"id":"1047","type":"CDSView"}},"id":"1046","type":"GlyphRenderer"},{"attributes":{"label":{"value":"Infected"},"renderers":[{"id":"1112","type":"GlyphRenderer"}]},"id":"1121","type":"LegendItem"},{"attributes":{"children":[{"id":"1355","type":"ToolbarBox"},{"id":"1353","type":"GridBox"}]},"id":"1356","type":"Column"},{"attributes":{},"id":"1023","type":"SaveTool"},{"attributes":{"data_source":{"id":"1106","type":"ColumnDataSource"},"glyph":{"id":"1109","type":"Line"},"hover_glyph":null,"muted_glyph":{"id":"1111","type":"Line"},"nonselection_glyph":{"id":"1110","type":"Line"},"selection_glyph":null,"view":{"id":"1113","type":"CDSView"}},"id":"1112","type":"GlyphRenderer"},{"attributes":{"active_drag":"auto","active_inspect":"auto","active_multi":null,"active_scroll":"auto","active_tap":"auto","tools":[{"id":"1091","type":"SaveTool"},{"id":"1092","type":"PanTool"},{"id":"1093","type":"WheelZoomTool"},{"id":"1094","type":"BoxZoomTool"},{"id":"1095","type":"ResetTool"}]},"id":"1096","type":"Toolbar"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"field":"node_color","transform":{"id":"1037","type":"CategoricalColorMapper"}},"line_alpha":{"value":0.2},"size":{"units":"screen","value":15}},"id":"1042","type":"Circle"},{"attributes":{"callback":null,"renderers":[{"id":"1053","type":"GraphRenderer"}],"tags":["hv_created"],"tooltips":[["index","@{index_hover}"],["bipartite","@{bipartite}"],["degree_centrality","@{degree_centrality}"],["infected","@{infected}"],["recovered","@{recovered}"],["status","@{status}"]]},"id":"1003","type":"HoverTool"},{"attributes":{"line_alpha":0.2,"line_color":"green","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1127","type":"Line"},{"attributes":{"dimension":1,"grid_line_color":{"value":null},"ticker":{"id":"1019","type":"BasicTicker"}},"id":"1022","type":"Grid"},{"attributes":{"source":{"id":"1122","type":"ColumnDataSource"}},"id":"1129","type":"CDSView"},{"attributes":{},"id":"1019","type":"BasicTicker"},{"attributes":{"line_alpha":0.1,"line_color":"yellow","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1143","type":"Line"},{"attributes":{"line_alpha":0.1,"line_color":"green","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1126","type":"Line"},{"attributes":{"callback":null,"data":{"Time":{"__ndarray__":"AQAAAAIAAAADAAAABAAAAA==","dtype":"int32","shape":[4]},"Value":{"__ndarray__":"CwAAABEAAAAYAAAAGgAAAA==","dtype":"int32","shape":[4]}},"selected":{"id":"1123","type":"Selection"},"selection_policy":{"id":"1155","type":"UnionRenderers"}},"id":"1122","type":"ColumnDataSource"},{"attributes":{"data_source":{"id":"1122","type":"ColumnDataSource"},"glyph":{"id":"1125","type":"Line"},"hover_glyph":null,"muted_glyph":{"id":"1127","type":"Line"},"nonselection_glyph":{"id":"1126","type":"Line"},"selection_glyph":null,"view":{"id":"1129","type":"CDSView"}},"id":"1128","type":"GlyphRenderer"},{"attributes":{"callback":null,"end":28.6,"reset_end":28.6,"reset_start":-2.6,"start":-2.6,"tags":[[["Value","Value",null]]]},"id":"1071","type":"Range1d"},{"attributes":{},"id":"1337","type":"Selection"},{"attributes":{"callback":null,"end":4.3,"reset_end":4.3,"reset_start":0.7,"start":0.7,"tags":[[["Time","Time",null]]]},"id":"1070","type":"Range1d"},{"attributes":{},"id":"1014","type":"BasicTicker"},{"attributes":{"data_source":{"id":"1139","type":"ColumnDataSource"},"glyph":{"id":"1142","type":"Line"},"hover_glyph":null,"muted_glyph":{"id":"1144","type":"Line"},"nonselection_glyph":{"id":"1143","type":"Line"},"selection_glyph":null,"view":{"id":"1146","type":"CDSView"}},"id":"1145","type":"GlyphRenderer"},{"attributes":{},"id":"1168","type":"UnionRenderers"},{"attributes":{},"id":"1155","type":"UnionRenderers"},{"attributes":{"axis_label":"x","bounds":"auto","formatter":{"id":"1067","type":"BasicTickFormatter"},"major_label_orientation":"horizontal","ticker":{"id":"1014","type":"BasicTicker"}},"id":"1013","type":"LinearAxis"},{"attributes":{},"id":"1077","type":"LinearScale"},{"attributes":{"axis_label":"Time","bounds":"auto","formatter":{"id":"1103","type":"BasicTickFormatter"},"major_label_orientation":"horizontal","ticker":{"id":"1082","type":"BasicTicker"}},"id":"1081","type":"LinearAxis"},{"attributes":{"line_width":{"value":2}},"id":"1048","type":"MultiLine"},{"attributes":{"data_source":{"id":"1039","type":"ColumnDataSource"},"glyph":{"id":"1048","type":"MultiLine"},"hover_glyph":{"id":"1050","type":"MultiLine"},"muted_glyph":null,"nonselection_glyph":{"id":"1049","type":"MultiLine"},"selection_glyph":null,"view":{"id":"1052","type":"CDSView"}},"id":"1051","type":"GlyphRenderer"},{"attributes":{},"id":"1107","type":"Selection"},{"attributes":{"line_color":"red","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1109","type":"Line"},{"attributes":{"line_alpha":{"value":0.2},"line_width":{"value":2}},"id":"1049","type":"MultiLine"},{"attributes":{"line_color":{"value":"limegreen"},"line_width":{"value":2}},"id":"1050","type":"MultiLine"},{"attributes":{},"id":"1079","type":"LinearScale"},{"attributes":{"line_color":"green","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1125","type":"Line"},{"attributes":{},"id":"1123","type":"Selection"},{"attributes":{"line_alpha":0.2,"line_color":"yellow","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1144","type":"Line"},{"attributes":{"below":[{"id":"1013","type":"LinearAxis"}],"center":[{"id":"1017","type":"Grid"},{"id":"1022","type":"Grid"}],"left":[{"id":"1018","type":"LinearAxis"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"plot_height":700,"plot_width":700,"renderers":[{"id":"1053","type":"GraphRenderer"}],"sizing_mode":"fixed","title":{"id":"1005","type":"Title"},"toolbar":{"id":"1029","type":"Toolbar"},"toolbar_location":null,"x_range":{"id":"1001","type":"Range1d"},"x_scale":{"id":"1009","type":"LinearScale"},"y_range":{"id":"1002","type":"Range1d"},"y_scale":{"id":"1011","type":"LinearScale"}},"id":"1004","subtype":"Figure","type":"Plot"},{"attributes":{"source":{"id":"1139","type":"ColumnDataSource"}},"id":"1146","type":"CDSView"},{"attributes":{"line_color":"yellow","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1142","type":"Line"},{"attributes":{"click_policy":"mute","items":[{"id":"1121","type":"LegendItem"},{"id":"1138","type":"LegendItem"},{"id":"1157","type":"LegendItem"}]},"id":"1120","type":"Legend"},{"attributes":{"bottom_units":"screen","fill_alpha":{"value":0.5},"fill_color":{"value":"lightgrey"},"left_units":"screen","level":"overlay","line_alpha":{"value":1.0},"line_color":{"value":"black"},"line_dash":[4,4],"line_width":{"value":2},"render_mode":"css","right_units":"screen","top_units":"screen"},"id":"1119","type":"BoxAnnotation"},{"attributes":{"text":"Metrics","text_color":{"value":"black"},"text_font_size":{"value":"12pt"}},"id":"1073","type":"Title"},{"attributes":{"source":{"id":"1106","type":"ColumnDataSource"}},"id":"1113","type":"CDSView"},{"attributes":{"label":{"value":"Susceptible"},"renderers":[{"id":"1145","type":"GlyphRenderer"}]},"id":"1157","type":"LegendItem"},{"attributes":{"line_alpha":0.1,"line_color":"red","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1110","type":"Line"},{"attributes":{"line_alpha":0.2,"line_color":"red","line_width":2,"x":{"field":"Time"},"y":{"field":"Value"}},"id":"1111","type":"Line"},{"attributes":{},"id":"1069","type":"BasicTickFormatter"},{"attributes":{},"id":"1336","type":"UnionRenderers"},{"attributes":{"grid_line_color":{"value":null},"ticker":{"id":"1082","type":"BasicTicker"}},"id":"1085","type":"Grid"},{"attributes":{"dimension":1,"grid_line_color":{"value":null},"ticker":{"id":"1087","type":"BasicTicker"}},"id":"1090","type":"Grid"},{"attributes":{"dimension":"height","level":"glyph","line_color":{"value":"#30a2da"},"line_width":{"value":3},"location":1},"id":"1244","type":"Span"},{"attributes":{"overlay":{"id":"1119","type":"BoxAnnotation"}},"id":"1094","type":"BoxZoomTool"},{"attributes":{"axis_label":"y","bounds":"auto","formatter":{"id":"1069","type":"BasicTickFormatter"},"major_label_orientation":"horizontal","ticker":{"id":"1019","type":"BasicTicker"}},"id":"1018","type":"LinearAxis"},{"attributes":{},"id":"1338","type":"UnionRenderers"},{"attributes":{},"id":"1082","type":"BasicTicker"},{"attributes":{},"id":"1335","type":"Selection"},{"attributes":{},"id":"1136","type":"UnionRenderers"},{"attributes":{"bottom_units":"screen","fill_alpha":{"value":0.5},"fill_color":{"value":"lightgrey"},"left_units":"screen","level":"overlay","line_alpha":{"value":1.0},"line_color":{"value":"black"},"line_dash":[4,4],"line_width":{"value":2},"render_mode":"css","right_units":"screen","top_units":"screen"},"id":"1334","type":"BoxAnnotation"},{"attributes":{},"id":"1092","type":"PanTool"},{"attributes":{"toolbar":{"id":"1354","type":"ProxyToolbar"},"toolbar_location":"above"},"id":"1355","type":"ToolbarBox"},{"attributes":{},"id":"1093","type":"WheelZoomTool"},{"attributes":{},"id":"1105","type":"BasicTickFormatter"},{"attributes":{"style":{"white-space":"nowrap"},"text":"<span style=\"color:black;font-family:Arial;font-style:bold;font-weight:bold;font-size:12pt\">Time: 1</span>","width":450},"id":"1357","type":"Div"},{"attributes":{},"id":"1091","type":"SaveTool"},{"attributes":{"axis_label":"Value","bounds":"auto","formatter":{"id":"1105","type":"BasicTickFormatter"},"major_label_orientation":"horizontal","ticker":{"id":"1087","type":"BasicTicker"}},"id":"1086","type":"LinearAxis"},{"attributes":{},"id":"1087","type":"BasicTicker"},{"attributes":{"factors":["R","S","I"],"palette":["green","yellow","red"]},"id":"1037","type":"CategoricalColorMapper"},{"attributes":{"callback":null,"data":{"Time":{"__ndarray__":"AQAAAAIAAAADAAAABAAAAA==","dtype":"int32","shape":[4]},"Value":{"__ndarray__":"DwAAAAgAAAAGAAAABgAAAA==","dtype":"int32","shape":[4]}},"selected":{"id":"1140","type":"Selection"},"selection_policy":{"id":"1168","type":"UnionRenderers"}},"id":"1139","type":"ColumnDataSource"},{"attributes":{"label":{"value":"Recovered"},"renderers":[{"id":"1128","type":"GlyphRenderer"}]},"id":"1138","type":"LegendItem"},{"attributes":{},"id":"1103","type":"BasicTickFormatter"},{"attributes":{},"id":"1140","type":"Selection"},{"attributes":{"tools":[{"id":"1003","type":"HoverTool"},{"id":"1023","type":"SaveTool"},{"id":"1024","type":"PanTool"},{"id":"1025","type":"WheelZoomTool"},{"id":"1026","type":"BoxZoomTool"},{"id":"1027","type":"ResetTool"},{"id":"1028","type":"TapTool"},{"id":"1091","type":"SaveTool"},{"id":"1092","type":"PanTool"},{"id":"1093","type":"WheelZoomTool"},{"id":"1094","type":"BoxZoomTool"},{"id":"1095","type":"ResetTool"}]},"id":"1354","type":"ProxyToolbar"},{"attributes":{"below":[{"id":"1081","type":"LinearAxis"}],"center":[{"id":"1085","type":"Grid"},{"id":"1090","type":"Grid"},{"id":"1120","type":"Legend"}],"left":[{"id":"1086","type":"LinearAxis"}],"margin":null,"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"plot_height":400,"plot_width":400,"renderers":[{"id":"1112","type":"GlyphRenderer"},{"id":"1128","type":"GlyphRenderer"},{"id":"1145","type":"GlyphRenderer"},{"id":"1244","type":"Span"}],"sizing_mode":"fixed","title":{"id":"1073","type":"Title"},"toolbar":{"id":"1096","type":"Toolbar"},"toolbar_location":null,"x_range":{"id":"1070","type":"Range1d"},"x_scale":{"id":"1077","type":"LinearScale"},"y_range":{"id":"1071","type":"Range1d"},"y_scale":{"id":"1079","type":"LinearScale"}},"id":"1072","subtype":"Figure","type":"Plot"}],"root_ids":["1358"]},"title":"Bokeh Application","version":"1.1.0"}};
  var render_items = [{"docid":"0149d744-b9c8-44b6-b64e-9b1861cb3af6","roots":{"1358":"1c360703-a09e-4f30-acb3-279a51b55767"}}];
  root.Bokeh.embed.embed_items_notebook(docs_json, render_items);

  }
  if (root.Bokeh !== undefined) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined) {
        embed_document(root);
        clearInterval(timer);
      }
      attempts++;
      if (attempts > 100) {
        console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        clearInterval(timer);
      }
    }, 10, root)
  }
})(window);</script>
  </body>
</html>
diameter: 4<br>density: 0.08971774193548387<br>
---
End of code
