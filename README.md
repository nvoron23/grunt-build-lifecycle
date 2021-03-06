# grunt-build-lifecycle

> Flexible build lifecycles for grunt

[![Build Status](https://travis-ci.org/wmluke/grunt-build-lifecycle.png)](https://travis-ci.org/wmluke/grunt-build-lifecycle)
[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/wmluke/grunt-build-lifecycle/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-build-lifecycle --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
// Must run after `grunt.initConfig`
grunt.loadNpmTasks('grunt-build-lifecycle');
```

## The "lifecycle" task

Inspired by the [maven build lifecycle](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html), the `lifecycle` task allows you to tame your grunt tasks with a consistent build lifecycle.

This means that the process for compiling, testing, and packaging a particular project is clearly defined.  For the person building a project, this means that it is only necessary to learn a small set of commands to build any project, and the `lifecycle` task will ensure they get the results they desired.

### Example
In your project's Gruntfile, add a section named `lifecycle` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
    lifecycle: {
        validate: [
            'jshint',
            'csslint'
        ],
        compile: [
            'coffee',
            'compass'
        ],
        test: [
            'connect:test',
            'karma:unit'
        ],
        package: [
            'concat',
            'uglify'
        ],
        'integration-test': [
            'karma:e2e'
        ],
        verify: [],
        install: [],
        deploy: [
            'server'
        ]
    },

    // other tasks...
});

// Must run after `grunt.initConfig`
grunt.loadNpmTasks('grunt-build-lifecycle');
```

This will create the following grunt tasks: `validate`, `compile`, `test`, `package`, `integration-test`, `verify`, `install`, and `deploy`.  Running any of these tasks will run all the preceding lifecycle tasks sequentially.

For example, `grunt test` runs the `validate`, `compile`, and `test` tasks.

#### Phase Tasks

You can run build phases individually with `grunt phase-<name>`, where name is one of the lifecycles.

For example, based on the above example `grunt phase-compile` runs the `coffee` and `compass` tasks.

#### Skip that

Additionally, you can skip phases with the `--skip` parameter.

For example, `grunt install --skip=validate,test` will skip the `validate` and `test` phases.

To skip all tests phases, use `--skipMatch=test`.

#### Roll your own

Perhaps maven-ish build cycles are not your thing, you can define your own set of lifecycle phases.  Here's another example.  Just remember, consistency is a good thing.

```js
grunt.initConfig({
    lifecycle: {
        lint: [
            'jshint',
            'csslint'
        ],
        compile: [
            'coffee',
            'compass'
        ],
        unit-test: [
            'connect:test',
            'karma:unit'
        ],
        package: [
            'concat',
            'uglify'
        ],
        e2e-test: [
            'karma:e2e'
        ],
        run: [
            'server'
        ]
    },

    // other tasks...
});

// Must run after `grunt.initConfig`
grunt.loadNpmTasks('grunt-build-lifecycle');
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).
