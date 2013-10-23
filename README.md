# multi-profile [![NPM version](https://badge.fury.io/js/multi-profile.png)](http://badge.fury.io/js/multi-profile) [![Build Status](https://travis-ci.org/kaelzhang/node-multi-profile.png?branch=master)](https://travis-ci.org/kaelzhang/node-multi-profile) [![Dependency Status](https://gemnasium.com/kaelzhang/node-multi-profile.png)](https://gemnasium.com/kaelzhang/node-multi-profile)

Multi-profile is a module to manage global configurations for multiple profiles.

## Installation

```sh
npm install multi-profile --save
```

## Usage

```js
var multi_profile = require('multi-profile');
var data = {};
var profile = multi_profile({
    path: '~/.cortex',
    context: data,
    schema: {
        port: {
            value: 8888,
            type: {
                validator: function (v, key, attr) {
                    if(v <= 9000){
                        attr.error('`port` must greater than 9000');
                    }
                    // If you've already called `attr.error`, it will be considered a failure,
                    // which means it's not necessary to return a value
                    return v > 9000;
                },
                setter: function (v, key, attr) {
                    // `this` <=> `data`
                    // The return value will set to the multi-profile instance.
                    // And in the same time, we set `port` to `data`
                    return this.port = v;
                },
                getter: function(v, key, attr){
                    return this.port || v;
                }
            }
        },

        rcfile: {
            value: '~/.bashrc'
        }
    }
});
```

## Instance Methods

### profile.add(name)

Add a profile

### profile.all()

Get all profile names

### profile.current()

Get the name of the current profile

### profile.switchTo(name)

Switch to a profile

### profile.del(name)

Delete a profile by name

### profile.set(key, value)

Set a configuration. If key is not in the `schema`, nothing will be done.

### profile.set(key_value_map)

Set a bunch of configurations

### profile.get()

Get all configurations of the curent profile

### profile.get(key)

Get the configuration of the current profile by key

### profile.save(data)

Save `data` to the configuration.

If `data` is not specified, all writable key-values will be saved.

## Static Methods

### multi_profile.resolveHomePath(path)

Resolve `'~/'` to the absolute home path of the curernt user.



