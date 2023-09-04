# Node Opera Driver

[![npm](https://img.shields.io/npm/dt/operadriver.svg)](https://www.npmjs.com/package/operadriver)

An NPM wrapper for Selenium [OperaDriver](https://sites.google.com/a/chromium.org/chromedriver/).

## Building and Installing

```shell
npm install operadriver
```

Or grab the source and

```shell
node ./install.js
```

What this is really doing is just grabbing a particular "blessed" (by
this module) version of OperaDriver. As new versions are released
and vetted, this module will be updated accordingly.

The package has been set up to fetch and run OperaDriver for MacOS (darwin),
Linux based platforms (as identified by Node.js), and Windows. If you
spot any platform weirdness, let us know or send a patch.

## Force download

By default this package, when installed, will search for an existing
OperaDriver binary in your configured temp directory. If found, and it is the
correct version, it will simply copy it to your node_modules directory. You can
force it always download by configuring it:

```shell
npm install operadriver --operadriver-force-download
```

Or add property into your [`.npmrc`](https://docs.npmjs.com/files/npmrc) file.

```
operadriver_force_download=true
```

Another option is to use PATH variable `OPERADRIVER_FORCE_DOWNLOAD`.

```shell
OPERADRIVER_FORCE_DOWNLOAD=true npm install operadriver
```

## Custom binaries url

To use a mirror of the OperaDriver binaries use npm config property `operadriver_cdnurl`.
Default is `https://github.com/operasoftware/operachromiumdriver/releases/download/`.

```shell
npm install operadriver --operadriver_cdnurl=https://github.com/operasoftware/operachromiumdriver/releases/download/
```

Or add property into your [`.npmrc`](https://docs.npmjs.com/files/npmrc) file.

```
operadriver_cdnurl=https://github.com/operasoftware/operachromiumdriver/releases/download/
```

Another option is to use PATH variable `OPERADRIVER_CDNURL`.

```shell
OPERADRIVER_CDNURL=https://github.com/operasoftware/operachromiumdriver/releases/download/ npm install operadriver
```

## Custom binaries file

To get the operadriver from the filesystem instead of a web request use the npm config property `OPERADRIVER_FILEPATH`.

```shell
npm install operadriver --OPERADRIVER_FILEPATH=/path/to/operadriver_mac64.zip
```

Or add property into your [`.npmrc`](https://docs.npmjs.com/files/npmrc) file.

```
OPERADRIVER_FILEPATH=/path/to/operadriver_mac64.zip
```

Another option is to use the PATH variable `OPERADRIVER_FILEPATH`

```shell
OPERADRIVER_FILEPATH=/path/to/operadriver_mac64.zip
```

This variable can be used to set either a `.zip` file or the binary itself, eg:

```shell
OPERADRIVER_FILEPATH=/bin/operadriver
```

## Custom download options

Install through a proxy.

```shell
npm config set proxy http://[user:pwd]@domain.tld:port
npm config set https-proxy http://[user:pwd]@domain.tld:port
```

Use different User-Agent.

```shell
npm config set user-agent "Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0"
```

## Skipping operadriver download

You may wish to skip the downloading of the operadriver binary file, for example if you know for certain that it is already there or if you want to use a system binary and just use this module as an interface to interact with it.

To achieve this you can use the npm config property `operadriver_skip_download`.

```shell
npm install operadriver --operadriver_skip_download=true
```

Or add property into your [`.npmrc`](https://docs.npmjs.com/files/npmrc) file.

```
operadriver_skip_download=true
```

Another option is to use the PATH variable `OPERADRIVER_SKIP_DOWNLOAD`

```shell
OPERADRIVER_SKIP_DOWNLOAD=true
```

## Running

```shell
bin/operadriver [arguments]
```

And npm will install a link to the binary in `node_modules/.bin` as
it is wont to do.

## Running with Selenium WebDriver

```javascript
require("operadriver");
var webdriver = require("selenium-webdriver");
var driver = new webdriver.Builder().forBrowser("chrome").build();
```

(Tested for selenium-webdriver version `2.48.2`)

The path will be added to the process automatically, you don't need to configure it.
But you can get it from `require('operadriver').path` if you want it.

## Running via node

The package exports a `path` string that contains the path to the
operadriver binary/executable.

Below is an example of using this package via node.

```javascript
var childProcess = require("child_process");
var chromiumoperadriver = require("operadriver");
var binPath = chromiumoperadriver.path;

var childArgs = ["some argument"];

childProcess.execFile(binPath, childArgs, function (err, stdout, stderr) {
  // handle results
});
```

You can also use the start and stop methods:

```javascript
var chromiumoperadriver = require("operadriver");

args = [
  // optional arguments
];
chromiumoperadriver.start(args);
// run your tests
chromiumoperadriver.stop();
```

With the latest version, you can optionally receive a Promise from the `chromiumoperadriver.start` function:

```javascript
var returnPromise = true;
chromiumoperadriver.start(args, returnPromise).then(() => {
  console.log("chromiumoperadriver is ready");
});
```

Note: if your tests are ran asynchronously, chromiumoperadriver.stop() will have to be
executed as a callback at the end of your tests

## Versioning

The NPM package version tracks the version of operadriver that will be installed,
with an additional build number that is used for revisions to the installer.
You can use the package version number to install a specific version, or use the
setting to a specific version. If there is a new Edge Driver version available which is not yet available as a version of `operadriver`, the npm command `npm run update-operadriver` in this repository can be used to make the required updates to this module, please submit the change as a PR. To always install the latest version of Operadriver,
use `LATEST` as the version number:

```shell
npm install operadriver --operadriver_version=LATEST
```

Or add property into your [`.npmrc`](https://docs.npmjs.com/files/npmrc) file.

```
operadriver_version=LATEST
```

Another option is to use env variable `OPERADRIVER_VERSION`.

```shell
OPERADRIVER_VERSION=LATEST npm install operadriver
```

## A Note on operadriver

Operadriver is not a library for NodeJS.

This is an _NPM wrapper_ and can be used to conveniently make Operadriver available.
It is not a Node.js wrapper.

## Supported Node.js versions

We will do our best to support every supported Node.js versions.
See [nodejs/Release](https://github.com/nodejs/Release) for
the current supported versions. You can also view our
[build scripts](https://github.com/uex-io/node-operadriver/blob/master/azure-pipelines.yml) and check the versions there.

## Contributing

Questions, comments, bug reports, and pull requests are all welcome. Submit them at
[the project on GitHub](https://github.com/giggio/node-chromedriver/).

Bug reports that include steps-to-reproduce (including code) are the
best. Even better, make them in the form of pull requests.

We have added
[VS Code Remote support with containers](https://code.visualstudio.com/docs/remote/containers).
If you are on Windows, set `git config core.autocrlf input` so you don't get git errors.

## Author

[Giovanni Bassi](https://github.com/giggio), with collaboration from
[lots of good people](https://github.com/giggio/node-chromedriver/graphs/contributors).

Thanks for Obvious and their PhantomJS project for heavy inspiration! Check their project on [Github](https://github.com/Obvious/phantomjs/tree/master/bin).

## License

Licensed under the Apache License, Version 2.0.
