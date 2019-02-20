# React-Native using NEM2-SDK 

This documentation is intended to solve dependancies issues of react-native when using NEM2-sdk, but is not limited to NEM2-sdk package. This method is appliable for any package that is using nodeJS built-in library, just so you know what you are doing.

>Since react-native will check dependancies during compile time, package existence checking is not possible during run time.
##
Steps to workaround:

Create a react-native project and change to the project directory

>Please follow the instructions to create a react-native project [here](https://facebook.github.io/react-native/docs/getting-started).
>
>Installing dependancy with `yarn` is not recommended, as I am not able to make it work when installing with `yarn`. If someone kind of happen to tested it out, please kindly let me know the result. Many Thanks~~!!!
##
Create new file named `rn-cli.config.js`, put in the code as below:

```javascript
var extraNodeModules = {
  crypto: require.resolve('crypto-browserify'),
  fs: require.resolve('react-native-fs'),
  http: require.resolve('stream-http'),
  https: require.resolve('https-browserify'),
  process: require.resolve('process/browser.js'),
  stream: require.resolve('readable-stream'),
  tls: require.resolve('tls-browserify'),
  net: require.resolve('net-browserify'),
  util: require.resolve('util/util.js'),
  punycode: require.resolve('punycode/'),
  url: require.resolve('react-native-url'),
  querystring: require.resolve('querystring-es3/'),
  zlib: require.resolve('react-zlib-js'),
  path: require.resolve('react-native-path'),
  vm: require.resolve('vm-browserify')
};

module.exports = {
  resolver: {
    extraNodeModules: extraNodeModules
  }
};
```
> What it does is resolve the packages that is built-in in nodejs to another packages
##
put in all required the dependancy in package.json, under dependancies:

```javascript
"assert": "^1.4.1",
"asyncstorage-down": "^4.1.2",
"browserify-zlib": "^0.1.4",
"buffer": "^5.2.1",
"crypto-browserify": "^3.12.0",
"events": "^3.0.0",
"https-browserify": "^1.0.0",
"nem2-sdk": "^0.10.1",
"net-browserify": "^0.2.4",
"process": "^0.11.0",
"punycode": "^1.2.4",
"querystring-es3": "^0.2.1",
"react-native-fs": "^2.13.3",
"react-native-level-fs": "^3.0.0",
"react-native-path": "0.0.5",
"react-native-url": "0.0.2",
"react-zlib-js": "^1.0.4",
"readable-stream": "^1.0.33",
"stream-browserify": "^1.0.0",
"stream-http": "^3.0.0",
"tls-browserify": "^0.2.2",
"url": "^0.11.0",
"vm-browserify": "^1.1.0"
```

##
install all the dependancies
```sh
npm install
```

##
add packager config file into `app.json`, include the config file which is the `rn-cli.config.js` file created above

add the code below in the app.json, inside the expo object

```javascript
"packagerOpts": {
    "projectRoots": "",
    "config": "rn-cli.config.js"
},
```
##
run the command below

```sh
npm i --save-dev tradle/rn-nodeify
```

for linux
```sh
./node_modules/.bin/rn-nodeify --hack --install
```

for windows
```sh
.\node_modules\.bin\rn-nodeify --hack --install
```

after running the command, a `shim.js` file will be created

insert this code at the top of App.js
```javascript
import './shim.js'
```

##
run the following command with terminal or cmd, if the package `react-native-crypto` exist :

`Please remove it using yarn if the package is installed with yarn`

```sh
npm uninstall react-native-crypto

npm uninstall react-native-randombytes
```

> we need to remove react-native-crypto and react-native-randombytes
> as we are not using it, we are using crypto-browserify and react-native-randombytes throwing error

##
just import the NEM2-sdk with require
```javascript
const nem2 = require('nem2-sdk');
```
now you can use the NEM2-sdk without error, happy coding~!!

`This workaround is not 100% tested, some issues might happen when using the NEM2-sdk with react-native. Please feel free to file issues.`  


You can check out the js nem2-sdk [here](https://github.com/nemtech/nem2-sdk-typescript-javascript).  

NEM2 documentation and tutorials can be found [here](https://nemtech.github.io/getting-started/setup-workstation.html).

