# funk
**Funk.StoreBehavior**

Polymer high-fives Reflux

[![Build Status](https://travis-ci.org/devinivy/funk.svg?branch=master)](https://travis-ci.org/devinivy/funk)

## Usage
Funk marries Polymer and Reflux by allowing you to use standard Polymer v1 data-binding alongside Reflux's stores and actions.  It leverages the [`yarn-state-behavior`](https://github.com/yarn-co/yarn-state-behavior) to share state throughout your app.
  - Your **view** layer is Polymer.
  - Your **actions** are Reflux actions.
  - Your **stores** are Polymerized Reflux stores.
    - Views subscribe to stores via **Polymer data-binding!**

view.html
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

actions.html
```html
<link rel="import" href="bower_components/funk/funk.html">

<script>
  var MyActions = Funk.Reflux.createActions([
    'smellFinger'
  ]);
</script>
```

store.html
```html
<link rel="import" href="bower_components/polymer/polymer.html">
<link rel="import" href="bower_components/funk/funk.html">
<link rel="import" href="actions.html">

<script>
Polymer({

  is: 'my-store',

  behaviors: [
    Funk.StoreBehavior
  ],

  properties: {

    storeName: {
      type: String,
      value: 'myStore'
    },

    finger: {
      type: String,
      notify: true
    }

  },

  funkListenables: MyActions,

  onSmellFinger: function(whichFinger) {
    this.finger = whichFinger;
  }

});
</script>
```

## Docs and Tests
The relative paths used in this project assume that `funk` will be included alongside its dependencies using bower.  So, to test this repo and view documentation, a small amount of massaging must be done.  Also, HTML imports require CORS support, so we must use a simple web server to view the tests and API-level documentation.
```bash
bower install
ln -s .. bower_components/funk
python -m SimpleHTTPServer 8000
# Now navigate to, for example,
# http://localhost:8000/bower_components/funk/
```

### Documentation
To view the API-level documentation for the Polymer behaviors, follow the process above then navigate to **http://localhost:8000/bower_components/funk/**.

### Tests
Tests are placed inside the `test` folder and can be accessed at **http://localhost:8000/bower_components/funk/test/**.
