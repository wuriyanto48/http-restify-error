- SIMPLE HTTP ERROR CUSTOM FOR RESTIFY

When you dealing with callback, sometime you confuse which error you need to handle.
Because you don't know where the error comes from.

USAGE:

-repository.js

```javascript

  'use strict'

  const httpError = require('http-restify-error');

  function find(id, cb){
    cb({error: httpError.ERROR_TYPE.NOT_FOUND, msg: `user with id ${id} not found`}, null);
  }

  module.exports = find;

```

-handler.js

```javascript

  'use strict';

  const httpError = require('http-restify-error');

  const find = require('./repository');

  function getExample(req, res, next){
    let id = 1;
    find(id, (err, data) => {
      res.send(httpError.error(err.error, err.msg));
    });
  }

  module.exports = {
    getExample: getExample
  };

```

-index.js

```javascript

  'use strict';

  const restify = require('restify');

  const handler = require('./handler');

  const server = restify.createServer({
    versions: "0.0.1",
    name: 'MyApp',
  });

  server.get('/', (req, res, next) => {
    res.send("Test........");
  });

  server.get('/error-test', handler.getExample);

  server.listen(3000);

```
 This package build from https://github.com/restify/errors
