# Development

## Requirements

Building igv.js and running the examples require Linux or MacOS.  Other Unix environments will probably
work but have not been tested.

Windows users can use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

## Building

Building igv.js and running the examples requires [node.js](https://nodejs.org/).


```  
git clone https://github.com/igvteam/igv.js.git
cd igv.js
npm install
npm run build
```

This creates a dist folder with the following files

* igv.js - UMDS file for script include, AMD, or CJS modules.  A script include will define an "igv" global.
* igv.min.js - minified version of igv.js
* igv.esm.js --  ES6 module
* igv.esm.min.js --  minified version of igv.esm.js

## Tests

To run the tests from the command line

```
npm run test
```


## Examples

To run the examples install [http-server](https://www.npmjs.com/package/http-server).

Start  http-server from the project root directory

```bash
npx http-server 
```

Then open [http://localhost:8080/examples](http://localhost:8080/examples) in a web browser.


## Supported Browsers

igv.js require a modern web browser with support for Javascript ECMAScript 2015 (ES6).

## License

igv.js is [MIT](/LICENSE) licensed.
