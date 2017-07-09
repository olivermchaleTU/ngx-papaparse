# Angular 2/4 wrapper for Papa Parse

<a href="https://www.npmjs.com/package/ngx-papaparse">
    <img src="https://img.shields.io/npm/v/ngx-papaparse.svg" alt="npm version" height="18">
</a>
<a href="https://github.com/Alberthaff/ngx-papaparse/blob/master/LICENSE.md">
    <img src="https://img.shields.io/npm/l/ngx-papaparse.svg" alt="npm version" height="18">
</a>
<a href="https://www.npmjs.com/package/ngx-papaparse">
    <img src="https://img.shields.io/npm/dt/ngx-papaparse.svg" alt="npm version" height="18">
</a>


This is an Angular wrapper library for the [Papa Parse](https://github.com/mholt/PapaParse) CSV parser.



## Building the library
This step is only required if you intend to modify the library. Otherwise, just complete the installation.

    npm install
    npm run build


## Installation
You can install the library with [npm](https://npmjs.com).

    npm install ngx-papaparse --save 


## Getting started

### Load the library in Angular
You can import the library in your app.module.ts or any other module.
```javascript
import { PapaParseModule } from 'ngx-papaparse';

@NgModule({
  ...
  imports: [
    ...
    PapaParseModule
  ]
})
```

### Using the library
#### Parsing CSV to an array.
Following example converts CSV to an array.
```javascript
import { Component } from '@angular/core';
import { PapaParseService } from 'ngx-papaparse';

@Component({
  ...
})
export class AppComponent {

    constructor(private papa: PapaParseService) {
        let csvData = '"Hello","World!"';
        
        console.log(this.papa.parse(csvData,{
            complete: (results, file) => {
                alert("The data has been parsed!");
                console.log("Parsed: ", results, file);
            }
        }));
    }
}
```

#### Parsing an array to CSV.
Following example converts an array to CSV.
```javascript
import { Component } from '@angular/core';
import { PapaParseService } from 'ngx-papaparse';

@Component({
  ...
})
export class AppComponent {

    constructor(private papa: PapaParseService) {
        let data = ["Helloe", "World!"];
        
        console.log(this.papa.unparse(data,{
            complete: (results, file) => {
                alert("The data has been parsed!");
                console.log("Parsed: ", results, file);
            }
        }));
    }
}
```

#### Configuration
The second paramter in `papa.parse()` and `papa.unparse()` contains the configuration.

There are following options available:

| Option            | Type                          | Explanation   |
| ----------------- | ----------------------------- | ------------- |
| delimiter         | string or (input) => any      | The delimiting character. Leave blank to auto-detect. It can be a string or a function. If string, it must be one of length 1. If a function, it must accept the input as first parameter and it must return a string which will be used as delimiter. In both cases it cannot be found in papa.BAD_DELIMITERS. |  
| newline           | string                        | The newline sequence. Leave blank to auto-detect. Must be one of \r, \n, or \r\n. |
| quoteChar         | string                        | The character used to quote fields. The quoting of all fields is not mandatory. Any field which is not quoted will correctly read. |
| header            | boolean                       | If true, the first row of parsed data will be interpreted as field names. An array of field names will be returned in meta, and each row of data will be an object of values keyed by field name instead of a simple array. Rows with a different number of fields from the header row will produce an error. Warning: Duplicate field names will overwrite values in previous fields having the same name. |
| dynamicTyping     | boolean                       | If true, numeric and boolean data will be converted to their type instead of remaining strings. Numeric data must conform to the definition of a decimal literal. European-formatted numbers must have commas and dots swapped. If also accepts an object or a function. If object it's values should be a boolean to indicate if dynamic typing should be applied for each column number (or header name if using headers). If it's a function, it should return a boolean value for each field number (or name if using headers) which will be passed as first argument. |
| preview           | number                        | If > 0, only that many rows will be parsed. |
| encoding          | ?                             | The encoding to use when opening local files. If specified, it must be a value supported by the FileReader API. |
| worker            | boolean                       | 
| comments          | string                        | A string that indicates a comment (for example, "#" or "//"). When Papa encounters a line starting with this string, it will skip the line. |
| step              | callback                      | 
| complete          | callback                      | 
| error             | callback                      | A callback to execute if FileReader encounters an error. The function is passed two arguments: the error and the File. |
| download          | boolean                       | If true, this indicates that the string you passed as the first argument to `papa.parse()` is actually a URL from which to download a file and parse its contents. |
| skipEmptyLines    | boolean                       | If true, lines that are completely empty will be skipped. An empty line is defined to be one which evaluates to empty string. |
| chunk             | callback                      | A callback function, identical to step, which activates streaming. However, this function is executed after every chunk of the file is loaded and parsed rather than every row. Works only with local and remote files. Do not use both chunk and step callbacks together. For the function signature, see the documentation for the step function. |
| fastMode          | boolean                       | Fast mode speeds up parsing significantly for large inputs. However, it only works when the input has no quoted fields. Fast mode will automatically be enabled if no " characters appear in the input. You can force fast mode either way by setting it to true or false. |
| beforeFirstChunk  | callback                      | A function to execute before parsing the first chunk. Can be used with chunk or step streaming modes. The function receives as an argument the chunk about to be parsed, and it may return a modified chunk to parse. This is useful for stripping header lines (as long as the header fits in a single chunk). |
| withCredentials   | boolean                       | A boolean value passed directly into XMLHttpRequest's "withCredentials" property. |


