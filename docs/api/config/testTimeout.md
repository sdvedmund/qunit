---
layout: page-api
title: QUnit.config.testTimeout
excerpt: Default timeout for async tests.
groups:
  - config
redirect_from:
  - "/config/testTimeout/"
version_added: "1.0.0"
---

Default timeout in milliseconds after which an async test will fail. This helps to detect async tests that are broken, and prevents a test run from hanging indefinitely.

<table>
<tr>
  <th>type</th>
  <td markdown="span">`number`</td>
</tr>
<tr>
  <th>default</th>
  <td markdown="span">`3000`</td>
</tr>
</table>

Only async tests can timeout. An async test is any [QUnit.test](../QUnit/test.md) with an async function as callback, or that returns a Promise, or that calls [assert.async()](../assert/async.md).

Individual tests can override the default `testTimeout` config via [assert.timeout()](../assert/timeout.md) to any lower or higher amount.

It is recommended to set the default at `3000` or higher (3 seconds). A lower timeout may cause intermittent failures due to unrelated infrastructure delays that are known to sometimes occur inside CI services and other virtual servers.

## Deprecated: No timeout set

Starting in QUnit 2.21, a deprecation warning is logged if a test takes longer than 3 seconds, and there is no timeout set.

```
Test {name} took longer than 3000ms, but no timeout was set.
```

You can address this warning before upgrading to QUnit 3 as follows:

* **(Recommended)** Call [assert.timeout()](../assert/timeout.md) inside the slow test.

  ```js
  QUnit.test('example', function (assert) {
    assert.timeout(5000);
    // …
  });
  ```

* Or, set `QUnit.config.testTimeout` once from an [HTML or bootstrap script](../config/index.md).

  ```js
  QUnit.config.testTimeout = 60000; // 1 minute
  ```

* Or, set `qunit_config_testtimeout` via [preconfig](../config/index.md) as environment variables (for Node.js), or as global variables for HTML/browser environments (before QUnit is loaded).

* Or, your test runner of choice may offer other ways to set configuration.

  For example, set `testTimeout` via [karma-qunit](https://github.com/karma-runner/karma-qunit/#readme):

  ```js
  config.set({
    frameworks: ['qunit'],
    plugins: ['karma-qunit'],
    client: {
      qunit: {
        testTimeout: 5000
      }
    }
  });
  ```

## Introducing a default timeout

Prior to QUnit 3.0, there has not been a default timeout. This meant that a test could hang silently for many seconds or minutes before diagnostic details are presented (e.g. after a CI job reaches the maximum run time).

QUnit 3.0 changes the default timeout from undefined (Infinity) to 3 seconds.

## Changelog

| [QUnit 2.21.0](https://github.com/qunitjs/qunit/releases/tag/2.21.0) | Announce change of default from undefined to `3000`, with a deprecation warning.
