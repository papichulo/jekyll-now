---
layout: post
title: Implementing a simple javascript widget!
---
###Basic rules
* Don't clutter the global namespace
* Expose only necessary variables, preferrably in it's own contained object
* Check for prescence of dependencies before loading, if not present, load minified version of just the necessary stuff.
* Use JSONP to load content from other domains.

##JavaScript
```javascript
(window.ExampleWidget = window.ExampleWidget || function() {
	ExampleWidget.make = function(params)
	{
		var exampleWidget = {};
		var _containerElement;

		var make = function()
		{
			// ... check that jQuery is present, otherwise load.
			_containerElement = jQuery(params.containerSelector);
			instantiateWidget();
			return exampleWidget;
		};

		function instantiateWidget()
		{
			var url = "//example.com/GetWidgetHtml?callback=?;
			jQuery.getJSON(url, function(data){
				_containerElement.html(data.html);
			});
		}

		return make();
	};
})();
```

##HTML
```html
<script type="text/javascript" src="//example.com/example.widget.js"></script>
<script type="text/javascript">
	jQuery(function(){
		ExampleWidget.make({
			containerSelector: '#widgetContainer',
		});
	});
</script>
```

##Adding a StyleSheet
Adding a css-file can be done by just appending the link-tag to the head.
Something like this can be added as a function called from the make-function.
```javascript
var cssTag = jQuery("<link>", {
	rel: "stylesheet",
	type: "text/css",
	href: "//example.com/css/example.widget.css"
});
cssTag.appendTo('head');
```
Inspired by [Alex Marandon](http://alexmarandon.com/articles/web_widget_jquery/)
