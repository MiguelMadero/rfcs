- Start Date: 2016-04-24
- RFC PR: (leave this empty)
- ember-cli Issue: (leave this empty)

# Summary

Allows to `app.import` from node_modules in a similar way that we can import from `vendor` and `bower_components` today.

# Motivation

We want to encourage users to migrate off of bower. There are many client libraries
that already publish to NPM instead of or in addition to bower. If the library is
using [amd](http://requirejs.org/docs/whyamd.html),
[umd](https://github.com/umdjs/umd) or globals, we could `import` it in the same way
that we `import` bower components.

# Detailed design

Update `app.import` to allow the following code to work:

```
app.import('node_modules/lgtm/dist/lgtm.js');
```

The code above can now replace:

```
app.import(app.bowerDirectory + '/lgtm/dist/lgtm.js');
```

# Drawbacks

While this encourages users to move away from bower we're still relying on either globals, umd or amd. Some npm packages already support `jsnext:main`, but this
approach wouldn't support it yet. I think this could still be a step in the
right direction, since users can start to consume them from NPM and once
we are ready to process `jsnext`, we can warn and ask users to move there.
If they remain in bower, we wouldn't have an easy way to know that the
dependecy they are using already supports `jsnext`.

# Alternatives

* Not to encourage users to move off of bower until we have full support all types of
modules.
* Use ember-browserify. This requires a new way to import the module, migrating off of
bower requires more changes.
* Ask users to funnel the specific dependency into `vendor` so it can be imported.
* Ask users to write an addon that wraps the library to make it easier for others to
 consume.

# Unresolved questions

* Are there any build performance considerations by making the `node_modules` `import`able?
* How should we handle a case where a user tries to import from an addon?
* Do we need to do anything special to deal with nested node_modules?
