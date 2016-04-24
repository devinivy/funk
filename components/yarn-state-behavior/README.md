# yarn-state-behavior
**YarnBehaviors.StateBehavior**

Polymer v1 cross-component state

[![Build Status](https://travis-ci.org/yarn-co/yarn-state-behavior.svg?branch=master)](https://travis-ci.org/yarn-co/yarn-state-behavior)

## Docs and Tests
The relative paths used in this project assume that `yarn-state-behavior` will be included alongside its dependencies using bower.  So, to test this repo and view documentation, a small amount of massaging must be done.  Also, HTML imports require CORS support, so we must use a simple web server to view the tests and documentation.
```bash
bower install
ln -s .. bower_components/yarn-state-behavior
python -m SimpleHTTPServer 8000
# Now navigate to, for example,
# http://localhost:8000/bower_components/yarn-state-behavior/
```

### Documentation
To view the fancy documentation for this repo, follow the process above then navigate to **http://localhost:8000/bower_components/yarn-state-behavior/**.

### Tests
Tests are placed inside the `test` folder and can be accessed at **http://localhost:8000/bower_components/yarn-state-behavior/test/**.
