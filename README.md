# api documentation for  [babel-preset-stage-0 (v6.22.0)](https://babeljs.io/)  [![npm package](https://img.shields.io/npm/v/npmdoc-babel-preset-stage-0.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-babel-preset-stage-0) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-babel-preset-stage-0.svg)](https://travis-ci.org/npmdoc/node-npmdoc-babel-preset-stage-0)
#### Babel preset for stage 0 plugins

[![NPM](https://nodei.co/npm/babel-preset-stage-0.png?downloads=true)](https://www.npmjs.com/package/babel-preset-stage-0)

[![apidoc](https://npmdoc.github.io/node-npmdoc-babel-preset-stage-0/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-babel-preset-stage-0_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-babel-preset-stage-0/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-babel-preset-stage-0/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-babel-preset-stage-0/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Sebastian McKenzie",
        "email": "sebmck@gmail.com"
    },
    "dependencies": {
        "babel-plugin-transform-do-expressions": "^6.22.0",
        "babel-plugin-transform-function-bind": "^6.22.0",
        "babel-preset-stage-1": "^6.22.0"
    },
    "description": "Babel preset for stage 0 plugins",
    "devDependencies": {},
    "directories": {},
    "dist": {
        "shasum": "707eeb5b415da769eff9c42f4547f644f9296ef9",
        "tarball": "https://registry.npmjs.org/babel-preset-stage-0/-/babel-preset-stage-0-6.22.0.tgz"
    },
    "homepage": "https://babeljs.io/",
    "license": "MIT",
    "main": "lib/index.js",
    "maintainers": [
        {
            "name": "amasad",
            "email": "amjad.masad@gmail.com"
        },
        {
            "name": "hzoo",
            "email": "hi@henryzoo.com"
        },
        {
            "name": "jmm",
            "email": "npm-public@jessemccarthy.net"
        },
        {
            "name": "loganfsmyth",
            "email": "loganfsmyth@gmail.com"
        },
        {
            "name": "sebmck",
            "email": "sebmck@gmail.com"
        },
        {
            "name": "thejameskyle",
            "email": "me@thejameskyle.com"
        }
    ],
    "name": "babel-preset-stage-0",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "https://github.com/babel/babel/tree/master/packages/babel-preset-stage-0"
    },
    "scripts": {},
    "version": "6.22.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module babel-preset-stage-0](#apidoc.module.babel-preset-stage-0)
1.  object <span class="apidocSignatureSpan">babel-preset-stage-0.</span>plugins
1.  object <span class="apidocSignatureSpan">babel-preset-stage-0.</span>presets

#### [module babel-preset-stage-0.plugins](#apidoc.module.babel-preset-stage-0.plugins)
1.  [function <span class="apidocSignatureSpan">babel-preset-stage-0.plugins.</span>0 ()](#apidoc.element.babel-preset-stage-0.plugins.0)
1.  [function <span class="apidocSignatureSpan">babel-preset-stage-0.plugins.</span>1 (_ref)](#apidoc.element.babel-preset-stage-0.plugins.1)



# <a name="apidoc.module.babel-preset-stage-0"></a>[module babel-preset-stage-0](#apidoc.module.babel-preset-stage-0)



# <a name="apidoc.module.babel-preset-stage-0.plugins"></a>[module babel-preset-stage-0.plugins](#apidoc.module.babel-preset-stage-0.plugins)

#### <a name="apidoc.element.babel-preset-stage-0.plugins.0"></a>[function <span class="apidocSignatureSpan">babel-preset-stage-0.plugins.</span>0 ()](#apidoc.element.babel-preset-stage-0.plugins.0)
- description and source-code
```javascript
0 = function () {
  return {
    inherits: require("babel-plugin-syntax-do-expressions"),

    visitor: {
      DoExpression: function DoExpression(path) {
        var body = path.node.body.body;
        if (body.length) {
          path.replaceWithMultiple(body);
        } else {
          path.replaceWith(path.scope.buildUndefinedNode());
        }
      }
    }
  };
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.babel-preset-stage-0.plugins.1"></a>[function <span class="apidocSignatureSpan">babel-preset-stage-0.plugins.</span>1 (_ref)](#apidoc.element.babel-preset-stage-0.plugins.1)
- description and source-code
```javascript
1 = function (_ref) {
  var t = _ref.types;

  function getTempId(scope) {
    var id = scope.path.getData("functionBind");
    if (id) return id;

    id = scope.generateDeclaredUidIdentifier("context");
    return scope.path.setData("functionBind", id);
  }

  function getStaticContext(bind, scope) {
    var object = bind.object || bind.callee.object;
    return scope.isStatic(object) && object;
  }

  function inferBindContext(bind, scope) {
    var staticContext = getStaticContext(bind, scope);
    if (staticContext) return staticContext;

    var tempId = getTempId(scope);
    if (bind.object) {
      bind.callee = t.sequenceExpression([t.assignmentExpression("=", tempId, bind.object), bind.callee]);
    } else {
      bind.callee.object = t.assignmentExpression("=", tempId, bind.callee.object);
    }
    return tempId;
  }

  return {
    inherits: require("babel-plugin-syntax-function-bind"),

    visitor: {
      CallExpression: function CallExpression(_ref2) {
        var node = _ref2.node,
            scope = _ref2.scope;

        var bind = node.callee;
        if (!t.isBindExpression(bind)) return;

        var context = inferBindContext(bind, scope);
        node.callee = t.memberExpression(bind.callee, t.identifier("call"));
        node.arguments.unshift(context);
      },
      BindExpression: function BindExpression(path) {
        var node = path.node,
            scope = path.scope;

        var context = inferBindContext(node, scope);
        path.replaceWith(t.callExpression(t.memberExpression(node.callee, t.identifier("bind")), [context]));
      }
    }
  };
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
