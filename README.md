# crud_localStorage
localStorage CRUD(create, read, update, delete) API, to mock restful api before real backend api is ready.

[demo](https://cdn.rawgit.com/jdk137/crud_localStorage/master/index.html)

```
;(function () {
  var records = JSON.parse(localStorage.records || '[]');
  var createRecord = function (param, callback) {
    var record = $.extend(true, {}, param);
    record.id = 'record' + Math.random();
    records.push(record);
    localStorage.records = JSON.stringify(records);
    callback({
      id: record.id
    });
  };
  var updateRecord = function (param, callback) {
    var id = param.id;
    var record = records.filter(function (d) {
      return d.id === id;
    })[0];
    if (record) {
      record.name = param.name;
      record.data = param.data;
      localStorage.records = JSON.stringify(records);
      callback({});
    } else {
      callback({error: 'no record'});
    }
  };
  var deleteRecord = function (param, callback) {
    var id = param.id;
    var l = records.length;
    records = records.filter(function (d, i) {
      return d.id !== id;
    });
    if (records.length === l - 1) {
      localStorage.records = JSON.stringify(records);
      callback({});
    } else {
      callback({error: 'no record'});
    }
  };
  var readRecord = function (param, callback) {
    var id = param.id;
    var record = records.filter(function (d, i) {
      return d.id === id;
    })[0];
    if (record) {
      callback(record);
    } else {
      callback({error: 'no record'});
    }
  };
  var readRecords = function (callback) {
    callback($.extend([], records));
  };

  window.localStorageCRUD = {
    create: createRecord,
    read: readRecord,
    update: updateRecord,
    delete: deleteRecord,
    readAll: readRecords
  };

})();
```