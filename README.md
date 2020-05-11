# pdf-lecture

**Javascript cross-platform module to extract texts from PDFs.**

[![version](https://img.shields.io/npm/v/pdf-lecture.svg)](https://www.npmjs.org/package/pdf-lecture)
[![downloads](https://img.shields.io/npm/dt/pdf-lecture.svg)](https://www.npmjs.org/package/pdf-lecture)

## Similar Packages
* [pdf2json](https://www.npmjs.com/package/pdf2json) buggy, no support anymore, memory leak, throws non-catchable fatal errors
* [j-pdfjson](https://www.npmjs.com/package/j-pdfjson) fork of pdf2json
* [pdf-parser](https://github.com/dunso/pdf-parse) buggy, no tests
* [pdfreader](https://www.npmjs.com/package/pdfreader) using pdf2json
* [pdf-extract](https://www.npmjs.com/package/pdf-extract) not cross-platform using xpdf

## Installation
`npm install pdf-lecture`
 
## Basic Usage - Local Files

```js
var path = require("path");
var fs = require("fs");
var filePath = path.join(__dirname, "..your path");
var PDF = require("pdf-lecture");

PDF(filePath).then((data) => {
   console.log(data.numpages)
   console.log(data.text)
   console.log(data.pageTextArray)
});
```

## Exception Handling

```js
const fs = require('fs');

var filePath = path.join(__dirname, "..your path");
var PDF = require("pdf-lecture");

PDF(filePath).then(function(data) {
	// use data
})
.catch(function(error){
	// handle exceptions
})
```

```js
// default render callback
function render_page(pageData) {
  let render_options = {
    normalizeWhitespace: false,
    disableCombineTextItems: false,
  };

  return pageData.getTextContent(render_options).then(function (textContent) {
    let lastY,
      text = "";
    for (let item of textContent.items) {
      if (lastY == item.transform[5] || !lastY) {
        text += item.str;
      } else {
        text += "\n" + item.str;
      }
      lastY = item.transform[5];
    }
    return text;
  });
}

let options = {
    pagerender: render_page
}

let dataBuffer = fs.readFileSync('path to PDF file...');

pdf(dataBuffer,options).then(function(data) {
	//use new format
});
```

## Options

```js
const DEFAULT_OPTIONS = {
	// internal page parser callback
	// you can set this option, if you need another format except raw text
	pagerender: render_page,
	
	// max page number to parse
	max: 0,
	
	//check https://mozilla.github.io/pdf.js/getting_started/
	version: 'v1.10.100'
}
```
### *pagerender* (callback)
If you need another format except raw text.  

### *max* (number)
Max number of page to parse. If the value is less than or equal to 0, parser renders all pages.  

### *version* (string, pdf.js version)
check [pdf.js](https://mozilla.github.io/pdf.js/getting_started/)

* `'default'`
* `'v1.9.426'`
* `'v1.10.100'`
* `'v1.10.88'`
* `'v2.0.550'`

>*default* version is *v1.10.100*   
>[mozilla.github.io/pdf.js](https://mozilla.github.io/pdf.js/getting_started/#download)


### Submitting an Issue
If you find a bug or a mistake, you can help by submitting an issue to [Github Repository](https://github.com/dulajkavinda/pdf-lecture/issues)

## License
[MIT licensed]
