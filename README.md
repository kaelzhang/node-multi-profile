# multi-profile [![NPM version](https://badge.fury.io/js/multi-profile.png)](http://badge.fury.io/js/multi-profile) [![Build Status](https://travis-ci.org/kaelzhang/node-multi-profile.png?branch=master)](https://travis-ci.org/kaelzhang/node-multi-profile) [![Dependency Status](https://gemnasium.com/kaelzhang/node-multi-profile.png)](https://gemnasium.com/kaelzhang/node-multi-profile)

Multi-profile is a module to manage global configurations for multiple profiles.

## Installation

```sh
npm install multi-profile --save
```

## Usage

```js
var profile = require('multi-profile');
var data = {};
var p = profile({
  path: '~/.cortex',
  context: data,
  schema: {
    port: {
      value: 8888,
      type: {
        validator: function(v, key, attr) {
          if (v <= 9000) {
            attr.error('`port` must greater than 9000');
          }
          // If you've already called `attr.error`, it will be considered a failure,
          // which means it's not necessary to return a value
          return v > 9000;
        },
        setter: function(v, key, attr) {
          // `this` <=> `data`
          // The return value will set to the multi-profile instance.
          // And in the same time, we set `port` to `data`
          return this.port = v;
        },
        getter: function(v, key, attr) {
          return this.port || v;
        }
      }
    },

    rcfile: {
      value: '~/.bashrc'
    }
  }
});

// .init() method must be called before any invocations of other methods.
p.init();
```

## profile

## Instance Methods

### .add(name)

Add a profile

### .all()

Get all profile names

### .current()

Get the name of the current profile

### .switchTo(name)

Switch to a profile

### .del(name)

Delete a profile by name

### .set(key, value)

Set a configuration. If key is not in the `schema`, nothing will be done.

### .set(key_value_map)

Set a bunch of configurations

### .get()

Get all configurations of the curent profile

### .get(key)

Get the configuration of the current profile by key

### .getConfigFile()

Returns `path` the path of the configuration file.

### .save(data)

Save `data` to the configuration.

If `data` is not specified, all writable key-values will be saved.

## Static Methods

### profile.resolveHomePath(path)

Resolve `'~/'` to the absolute home path of the curernt user.



