# funk

Polymer high-fives Reflux

[![Build Status](https://travis-ci.org/devinivy/funk.svg?branch=master)](https://travis-ci.org/devinivy/funk)

## Usage
Funk marries Polymer and Reflux by allowing you to use standard Polymer v1 data-binding alongside Reflux's stores and actions.  It leverages the [`yarn-state-behavior`](https://github.com/yarn-co/yarn-state-behavior) to share state throughout your app.
  - Your **view** layer is Polymer.
  - Your **actions** are Reflux actions.
  - Your **stores** are Polymerized Reflux stores.
    - Views subscribe to stores via **Polymer data-binding!**

### What You'll Find
#### `Funk.Reflux`
An alias for [reflux-core](https://github.com/reflux/reflux-core) (identical to [RefluxJS](https://github.com/reflux/refluxjs), but without the React helpers).  You'll find all the standard goodies on here like `Funk.Reflux.createActions()`.

#### `Funk.StoreBehavior`
The Polymer behavior used to create a Polymerized Reflux store.  Enables data-binding from the store to your views.

#### `Funk.ViewBehavior`
The Polymer behavior used to enable data-binding to a view from your stores.

#### `Funk.CollectionBehavior`
The Polymer behavior used by `Funk.StoreBehavior` to provide crucial helpers for working with lists and collections.

### Example
Check out the full [polymer-funky-flux](https://github.com/devinivy/polymer-funky-flux) demo or learn to use Funk by walking through this commented code!  If you're unfamiliar with Reflux or the flux pattern, start reading [here](https://github.com/reflux/refluxjs) to see how it will make your life better-er.

#### `view.html`
```html
<link rel="import" href="bower_components/polymer/polymer.html">
<link rel="import" href="bower_components/funk/funk.html">
<link rel="import" href="actions.html">

<dom-module id="my-view">
  <template>
    <!-- Look, a binding from myStore! -->
    <h1>Finger length: [[_length(state.myStore.finger)]]</h1>
    <ul on-click="_clickedFinger">
      <li>ring</li>
      <li>thumb</li>
      <li>pinky</li>
      <li>middle</li>
      <li>pointer</li>
    </ul>
  </template>
</dom-module>

<script>
Polymer({

  is: 'my-view',

  // Use the ViewBehavior to receive state changes from myStore
  behaviors: [
    Funk.ViewBehavior
  ],

  // Put MyActions to good use and give a whiff!
  _clickedFinger: function(e) {
    if (e.path[0].tagName === 'LI') {
      MyActions.smellFinger(e.path[0].innerHTML);
    }
  },

  _length: function(str) {
    return (str || '').length;
  }

});
</script>
```

#### `actions.html`
```html
<link rel="import" href="bower_components/funk/funk.html">

<script>
  // Funk.Reflux is just a reference to the Reflux library
  var MyActions = Funk.Reflux.createActions([
    'smellFinger'
  ]);
</script>
```

#### `store.html`
```html
<link rel="import" href="bower_components/polymer/polymer.html">
<link rel="import" href="bower_components/funk/funk.html">
<link rel="import" href="actions.html">

<script>
Polymer({

  is: 'my-store',

  // The StoreBehavior makes this element act
  // like a Reflux store... but in Polymer-land.
  behaviors: [
    Funk.StoreBehavior
  ],

  properties: {

    storeName: {
      type: String,
      value: 'myStore' // Name of the store as found on `state.myStore`
    },

    finger: {
      type: String,
      value: '',    // Initial finger state
      notify: true  // Indicates that this prop is part of app state
    }

  },

  // Subscribe to actions!
  // This works identically to Reflux's "listenables"
  funkListenables: MyActions,

  onSmellFinger: function(whichFinger) {
    // Setting finger here will notify
    // state.myStore.finger across your app
    this.finger = whichFinger;
  }

});
</script>
```

## Docs and Tests
[API docs](http://devinivy.github.io/funk) | [View demo](http://devinivy.github.io/funk/components/funk/demo/) | [Run tests](http://devinivy.github.io/funk/components/funk/test/)

### Locally
The relative paths used in this project assume that `funk` will be included alongside its dependencies using bower.  So, to test this repo and view documentation, a small amount of massaging must be done.  Also, HTML imports require CORS support, so we must use a simple web server to view the tests and API-level documentation.
```bash
bower install
ln -s .. bower_components/funk
python -m SimpleHTTPServer 8000
# Now navigate to, for example,
# http://localhost:8000/bower_components/funk/
```

#### Documentation
To view the API-level documentation for the Polymer behaviors, follow the process above then navigate to **http://localhost:8000/bower_components/funk/**.

#### Tests
Tests are placed inside the `test` folder and can be accessed at **http://localhost:8000/bower_components/funk/test/**.
