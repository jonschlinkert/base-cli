# base-cli [![NPM version](https://img.shields.io/npm/v/base-cli.svg)](https://www.npmjs.com/package/base-cli) [![Build Status](https://img.shields.io/travis/jonschlinkert/base-cli.svg)](https://travis-ci.org/jonschlinkert/base-cli)

> Plugin for base-methods that maps built-in methods to CLI args (also supports methods from a few plugins, like 'base-store', 'base-options' and 'base-data'.

## Install
Install with [npm](https://www.npmjs.com/):

```sh
$ npm i base-cli --save
```

Adds a `cli` method to `base` for mapping parsed command line arguments existing [base][] methods or custom functions. 

The goal is to simplify the process of settings up command line logic for your [base][] application.

## Usage

```js
var cli = require('base-cli');
var Base = require('base');
var app = new Base();

// register the plugin
app.use(cli());
```

## API

This adds a `cli` object to [base][] with the following (chainable) methods (`base.cli.*`):

- `.map()` -  [.map](#map): add mappings from command line flags/options to custom functions or `base` methods 
- `.alias()` -  [.alias](#alias): similar to `map` but creates simple aliases. For example, `alias('show', 'get')` would invoke the `.get()` method when `--show` is passed on the command line
- `.process()` -  [.process](#process): once all mappings are defined, pass `argv` to `.process()` to iterate over the mappings, passing `argv` as context.

## Example

```js
var argv = require('minimist')(process.argv.slice(2));
var expand = require('expand-args');
var cli = require('base-cli');
var Base = require('base');

var app = new Base();
app.use(cli());

app.cli
  .map('get', function(key, val) {
    app.get(key, val);
  })
  .map('set', function(key, val) {
    app.set(key, val);
  })

app.cli.process(expand(argv), function(err) {
  if (err) throw err;
});

// command line args:
//   
//   '--set=a:b --get=a'
//   
// prints:
//   
//   'a'
//   
```

### CLI

### [--ask](lib/commands/ask.js#L27)
Force questions that match the given pattern to be asked. The resulting answer data is merged onto `app.cache.data`.

After questions are answered:
- Use `app.data('answers')` to get answer data.
- To open the directory where data is persisted, enter `--open answers` in the command line

**Example**

```sh
# ask all questions
$ --ask
# ask all `author.*` questions
$ --ask "author.*"
# ask all `*.name` questions (like `project.name` and `author.name`)
$ --ask "*.name*"
```

### [--cli](lib/commands/cwd.js#L16)
Set the current working directory.

**Example**

```sh
$ --cwd=foo
```

### [--data](lib/commands/data.js#L22)
Set data on the `app.cache.data` object. This is the API-equivalent of calling `app.data()`.

**Example**

```sh
$ --data=foo
# sets {foo: true}
$ --data=foo:bar
# sets {foo: 'bar'}
$ --data=foo.bar:baz
# sets {foo:{bar: 'baz'}}
```

### [--emit](lib/commands/emit.js#L21)
Bind `console.error` to the given event listener, so that when event `name` is emitted, the event arguments will be output in the console.

**Example**

```sh
# emit errors
$ --emit error
# emit all views as they're created
$ --emit view
# emit only "pages" as they're created
$ --emit page
```

### [--open](lib/commands/open.js#L21)
Open a directory, or open a file in the default application associated with the file type.

**Example**

```sh
# Open the directory where answer data is persisted
$ --open answers
# Open the directory where store data is persisted
$ --open store
```

### [--option](lib/commands/option.js#L23)
Set options on the `app.options` object. This is the API-equivalent of calling `app.option()`. You may also use the plural `--options` flag for identical behavior.

**Example**

```sh
$ --option=foo
# sets {foo: true}
$ --option=foo:bar
# sets {foo: 'bar'}
$ --option=foo.bar:baz
# sets {foo:{bar: 'baz'}}
```

### [--tasks](lib/commands/tasks.js#L20)
Run the given generators and tasks. This flag is unnecessary when used with [base-runner][].

**Example**

```sh
$ app --tasks foo
# {tasks: ['foo']}
# Runs task "foo"
$ app --tasks foo:bar
# {tasks: ['foo:bar']}
# Runs generator "foo", task "bar"
```

## TODO

- [ ] implement `--init`
- [ ] implement `--config`
- [ ] implement `--verbose`
- [ ] implement short flags. do this last

## Related projects
Other useful [base][] plugins:
* [base](https://www.npmjs.com/package/base): base is the foundation for creating modular, unit testable and highly pluggable node.js applications, starting… [more](https://www.npmjs.com/package/base) | [homepage](https://github.com/node-base/base)
* [base-config](https://www.npmjs.com/package/base-config): base-methods plugin that adds a `config` method for mapping declarative configuration values to other 'base'… [more](https://www.npmjs.com/package/base-config) | [homepage](https://github.com/jonschlinkert/base-config)
* [base-data](https://www.npmjs.com/package/base-data): adds a `data` method to base-methods. | [homepage](https://github.com/jonschlinkert/base-data)
* [base-generators](https://www.npmjs.com/package/base-generators): Adds project-generator support to your `base` application. | [homepage](https://github.com/jonschlinkert/base-generators)
* [base-plugins](https://www.npmjs.com/package/base-plugins): Upgrade's plugin support in base-methods to allow plugins to be called any time after init. | [homepage](https://github.com/jonschlinkert/base-plugins)
* [base-task](https://www.npmjs.com/package/base-task): base plugin that provides a very thin wrapper around <https://github.com/doowb/composer> for adding task methods to… [more](https://www.npmjs.com/package/base-task) | [homepage](https://github.com/node-base/base-task)

## Running tests
Install dev dependencies:

```sh
$ npm i -d && npm test
```

## Contributing
Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](https://github.com/jonschlinkert/base-cli/issues/new).

## Author
**Jon Schlinkert**

+ [github/jonschlinkert](https://github.com/jonschlinkert)
+ [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

## License
Copyright © 2016 [Jon Schlinkert](https://github.com/jonschlinkert)
Released under the [MIT license](https://github.com/jonschlinkert/base-cli/blob/master/LICENSE).

***

_This file was generated by [verb](https://github.com/verbose/verb), v0.9.0, on February 09, 2016._

[base]: https://github.com/node-base/base

