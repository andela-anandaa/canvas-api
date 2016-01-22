# canvas-api

[![Build Status](https://travis-ci.org/neurotech/canvas-api.svg?branch=master)](https://travis-ci.org/neurotech/canvas-api)

*A Promise-based collection of helper methods for the [Canvas LMS](http://www.canvaslms.com/) API.*

## Installation

`npm install canvas-api --save`

## Configuration and Authorization

`canvas-api` expects some credentials for authenticating with your organization's Canvas instance. You can provide these credentials via the following environment variables (this is the preferred approach with regards to the security of your API key).

```
CANVAS_API_KEY=secret
CANVAS_API_DOMAIN=organization.instructure.com
```

## Usage/API

``` javascript
var canvas = require('canvas-api');
```

### SIS Imports

#### sis.status([config])

Return [SIS Import](https://canvas.instructure.com/doc/api/sis_imports.html#SisImport) status information.

Available options for `config` are:

##### config.domain

(Optional) Overrides the `CANVAS_API_DOMAIN` environment variable.

##### config.scope

 - If `latest` or if no `scope` is supplied, the latest SIS Import object will be returned.
 - If a SIS Import `id` is supplied, the SIS Import object with that id will be returned.
 - If `'all'` is supplied, all SIS Import objects will be returned.

###### Example

``` javascript
canvas.sis.status({scope: 5})
  .then(function (res) {
    // Log the SIS Import object with the ID of 5 to the console.
    console.log(res);
  }, function (err) {
    console.error(err);
  });
```

#### sis.upload([config])

POSTs a CSV file to the SIS Import endpoint that is formatted to match Instructure's [SIS CSV Format](https://canvas.instructure.com/doc/api/file.sis_csv.html). Upon success, a [SIS Import](https://canvas.instructure.com/doc/api/sis_imports.html#SisImport) is returned.

Available options for `config` are:

##### config.domain

(Optional) Overrides the `CANVAS_API_DOMAIN` environment variable.

##### config.csv

Must be a valid path to a CSV file, i.e. `'./csv/enrolments.csv'`.

##### config.dataset

A string that is used to apply the `diffing_data_set_identifier` to the POST request, i.e. `'enrolments'`.

###### Example

``` javascript
canvas.sis.upload({
  csv: './csv/enrolments.csv',
  dataset: 'enrolments'
})
  .then(function (res) {
    // Log the SIS Import object returned by Canvas to the console.
    console.log(res);
  }, function (err) {
    console.error(err);
  });
```

### Course Migration

#### migration.migrate([config])

Migrates the content of one course to another using the `course_copy_importer` migration type.

Available options for `config` are:

##### config.domain

(Optional) Overrides the `CANVAS_API_DOMAIN` environment variable.

##### config.from_id

ID number of the course that will be copied _**from**_.

##### config.to_id

ID number of the course that will be copied _**to**_.

###### Example

``` javascript
canvas.migration.migrate({
  from_id: 5,
  to_id: 42
})
  .then(function (res) {
    // Log the ContentMigration object returned by Canvas to the console.
    console.log(res);
  }, function (err) {
    console.error(err);
  });
```

---

## Breaking Changes

### > v1.0.3


#### SIS Imports

`sisStatus` and `sisUpload` are now subsumed into the `sis` object that this module exports.

i.e. `sisStatus` becomes `sis.status`

#### Authorization Keys

The ability to override the authorization key has been removed in favour of keeping sensitive credentials like that stored as environment variables.

---

## Tests

Tests can be run with `npm test` in the module's directory.

If you haven't set your environment variables yet, you can run the tests by supplying them via the command line:

`CANVAS_API_KEY=secret CANVAS_API_DOMAIN=organization.instructure.com npm test`
