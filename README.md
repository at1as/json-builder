# JSON-Builder

Visually create, explore or modify JSON payloads. This tool provides a way to safely change a particular value within a saved JSON file without needing a text editor. Used to quickly iterate on some content within [this book](https://www.amazon.com/Photography-Cookbook-Photographic-Recipes-Michael-ebook/dp/B01CUQ4L7O) (from which examples below are taken).


## Screenshot
<p align="center">
  <img src="http://at1as.github.io/github_repo_assets/json-builder-1.png" style="max-width:500px"/>
</p>

## Importing / Modifying JSON

Expects two pices of infomration: a JSON payload, and a defined schema

### 1 of 2 : JSON Data for Import

Expects a JSON document with an array of objects stored under a key. The structure should match the following format and a unique "id" field is highly recommended:

```
{
  "<global_key>" : [
    {"id": "1", "name": "first item!", ... },
    {"id": "2", "name": "second item!", ... },
    {"id": "3", "name": "third item!", ... },
    {"id": "4", "name": "forth item!", ... }
  ]
}
```

Values may themselves be nested objects. If a second top level key is given, underneath `global_key` (at the same level), the first key will be used by default. Otherwise the desired key to use can be set in the UI under the `Data Key` input.

#### JSON Data for Import - Example

```
{
  "recipes": [
    {
      "name": "Animals",
      "id": "1",
      "type": "indoor",
      "camera": {
        "mode": "Aperture, Program; Manual if using flash",
        "aperture setting": "f/5.6 - f/8",
        "iso": "200 - 800 ISO",
        "white balance": "",
        "shutter mode": "Continuous",
        "shutter speed": "If manual: 1/30th or faster",
        "focus points": "One focus point/area, selected by you",
        "focus mode": "If animal moves: AI Servo/AF-C",
        "flash compensation": "",
        "metering": "Evaluative/3D Color Matrix",
        "drive": "",
        "exposure compensation": "",
        "mount": ""
      },
      "lens": {
        "focal length": "Between 35 and 135 mm",
        "min. lens aperture": "Fast (Low \"F-number\"), just in case",
        "examples": [
          "24-70 f/2.8 (flexible)",
          "50mm f/1.8 (fast!)"
        ],
        "other": ""
      },
    },
  ]
}
```

For the moment this file to read data from MUST be titled `existing_json_data.json` and placed in the top-level-directory of this repo. This will be configurable later.


### 2 of 2 : JSON Schema

For the moment, JSON documents must be defined in a schema. It is difficult otherwise to infer the schema since objects may be empty.

The schema should follow the structure of the JSON document, defining three types:

* `"String"` (text data)
* `[]` (an array of elements)
* `{}` (a subobject with the keys and values defined within - see "camera" below)

Currently integers are not 



#### JSON Schema - Example:

```
{
  "recipes": [
    {
      "name": "String",
      "id": "String",
      "type": "String",
      "camera":{
        "mode": "String",
        "aperture setting": "String",
        "iso": "String",
      },
      "lens":{
        "focal length": "String",
        "examples": [],
      },
      "flash":{
        "type": "String",
        "settings": "String",
      },
      "other": [],
    }
  ]
}
```

This file must be titled `data.json`


## Building new JSON files

Same steps as above (with defining the schema), however the data import step is clearly not necessary.


### Running

```bash
$ git clone https://github.com/at1as/json-builder.git
$ cd json-builder
$ npm install

# Create a schema in data.json and 
# For importing data copy JSON data to json-builder/ folder and title it existing_json_data.json

$ npm start
```

### Operations
* Add Page (add a new JSON object at the end of the list – would create an Item 53 in the screenshot above)
* Delete Page (delete the currently displayed page. Will re-index IDs so they are sequential and do no skip numbers)
* Generate (save data to `json_output.json`)
* Import (import existing data from `existing_json_data.json`)
* Preview (preview JSON object in HTML table form to quickly get an overview)

### Notes

* Generated JSON objects will be stored in the json-builder/ directory with the name `json_output.json`
* Will save data periodically to `generated-backup.json` (on 'Add Page' or 'Delete Page' operations). However these backups are not rotated, so on the next such operation, the file will be overwritten. It's meant as a one-operation "undo" for mistakes.


### Disclaimer

* Built quickly for a specific task I had in a langauge that's not my first choice. Seems to work, but not exactly code I'm proud of. Tread carefully.


### TODO

* Visually display JSON as Table
* Test Schema with array of nested objects (unlikely to work)

