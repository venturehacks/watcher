watcher
=======
[![Build Status](https://travis-ci.org/venturehacks/watcher.svg)](https://travis-ci.org/venturehacks/watcher)

Watcher is a tiny opinionless Javascript initialization framework. It weighs about 1.2KB (compressed).

It currently requires `Promise` and `MutationObserver`, which are available natively on modern browsers, or via 
polyfill on older ones.

Usage
-----

Denote initializable blocks in your document with `data-watcher-name`:

```
<div data-watcher-name="toggle" />
  <a href="#" class="visible">Click me!</a>
  <a href="#" class="hidden">No, click ME!</a>
</div>
```

Create a `Watcher` instance and tell it how to initialize your blocks:

```
var Initializers = {
  // This function will be invoked once with each element whose data-watcher-name is "toggle"
  toggle: function(el) {
    $(el).on('click', 'a.visible', function() {
      var hidden = $(el).find('.hidden');
      $(el).find('.visible').removeClass('visible').addClass('hidden');
      hidden.removeClass('hidden').addClass('visible');
    });
  }
  
  foo: function(el) { 
    // ...
  }
};

new Watcher(function(name) {
  // Tell the watcher how to find an initialization function for blocks with the given name.
  return Initializers[name];
}).observe();
```

Now whenever you add another element with `data-watcher-name="toggle"` to the DOM, it will be automatically initialized:

```
var content = $.get(someUrl); // Contains a "toggle" block
$('body').append(content); // "toggle" block is initialized automatically
```

Motivation
----------

The world is filled with excellent Javascript frameworks, many of them full-featured and highly structured. Watcher is
extremely minimal, simply managing structured initialization of your components and allowing them to communicate with
each other. Perfect for loosely coupled applications at any scale, or applications that prefer to define their own
structure and toolkit.

- **Automatic, tightly scoped initialization**

  Associate HTML elements directly with their Javascript behaviors, regardless of when or where they were added to the
  DOM. Easily isolate the initialization of components on the page.

- **Clean communication between components**

  Expose controllers for the logical pieces of your interface, and easily give them access to one another. Controllers
  can be built asynchronously, and can be anything from plain objects to Backbone views to Ember controllers.

