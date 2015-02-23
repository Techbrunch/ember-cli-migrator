ember-cli-migrator
==================

# Installation

`npm install -g ember-cli-migrator`

# About

Migrate your files to the standard ember-cli structure, preserving git history.

You can run the command line tool by running the ember-cli-migrator script from within your existing ember project.

The goal of the project is to convert global variables to ES6 Modules. For example:

```javascript
App.Post = DS.Model.extend({

});
```
becomes

```javascript
import DS from "ember-data";

var Post = DS.Model.extend({

});

export default Post;
```
# Features
- The following known module types are currently handled:
  - Controllers
  - Routes
  - Views
  - Models
  - Mixins
  - Transforms
  - Adapters
  - Serializers
- Converts file names for you and puts them into the canonical ember CLI folders.
- Multiple modules in your current file will be split into their own export module and potentially into different folders. For example if you had a model and serializer defined in the same file, the migrator would split the serializer into a file in the serializers folder and the model module would exist in the models folder.
- Unknown types, e.g., a utility function defined on your app namespace, is exported in a module that is left in the location it was found.
- Non JS files, e.g., handlebars, erb, are moved to a nonJS folder with the same subdirectory hierarchy so they are easy for you to isolate and handle manually.

# Command Line Options

```
-h, --help                       output usage information
-V, --version                    output the version number
-g, --global [name]              Global namespace of Ember application, eg: "MyApplication = Ember.Application.."
-d, --directory [name]           Location of Ember application codebase
-s, --source [source_directory]  Directory to perform migration on
-t, --target [target_directory]  Directory to output result of migration
```

# Testing
You can run the tests by running `mocha` in the root folder.

# Dependencies
The project uses [recast](https://github.com/benjamn/recast) (which uses Esprima) to walk the JavaScript AST to accurately identify exports and move the file.

# Necessary Manual Steps
- App.Router = Ember.Router.extend(); placed at beginning of router file
- import ./config/environment in router
- move injectors & registers to initializers rather than on App
- have to merge app.js code, imports etc.
- move ENV stuff to config/environments.js
- var ObjectTransform = var ArrayTransform = Ember.Transform.extend

# TODOS
- [ ] Analyst.FindPageMixin = Em.Mixin.create(Analyst.PageMixin, {
- [ ] helpers
- [ ] use new visitor syntax
- [ ] App.Something = App.SomethingElse = Ember.Object.extend();
