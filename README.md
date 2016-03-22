# feathers-stripe

[![Build Status](https://travis-ci.org/feathersjs/feathers-stripe.png?branch=master)](https://travis-ci.org/feathersjs/feathers-stripe)

> A Feathers service for Stripe

**This is still a work in progress and is not ready for production.** Pull requests welcome! :smile:

## Installation

```
npm install feathers-stripe --save
```

## Documentation

Still a work in progress.

## Complete Example

Here's an example of a Feathers server that uses `feathers-authentication` for local auth.  It includes a `users` service that uses `feathers-mongoose`.  *Note that it does NOT implement any authorization.*

```js
var feathers = require('feathers');
var rest = require('feathers-rest');
var socketio = require('feathers-socketio');
var hooks = require('feathers-hooks');
var bodyParser = require('body-parser');
var errorHandler = require('feathers-errors/handler');
var stripe = require('../lib/index');

// Initialize the application
var app = feathers()
  .configure(rest())
  .configure(socketio())
  .configure(hooks())
  // Needed for parsing bodies (login)
  .use(bodyParser.json())
  .use(bodyParser.urlencoded({ extended: true }))
  // A simple Message service that we can used for testing
  .use('/stripe/charges', stripe.charge({ apiKey: 'your api key' }))
  .use('/', feathers.static(__dirname + '/public'))
  .use(errorHandler({ html: false }));


function validateCharge() {
  return function(hook) {
    console.log('Validating charge code goes here');
  };
}


var cardService = app.service('stripe/charges');

cardService.before({
  create: [validateCharge()]
});

var Charge = {
  amount: 400,
  currency: "cad",
  source: "tok_87rau6axWXeqLq", // obtained with Stripe.js
  description: "Charge for test@example.com"
};

cardService.create(Charge).then(result => {
  console.log('Charge created', result);
}).catch(error => {
  console.log('Error creating charge', error);
});

app.listen(3030);

console.log('Feathers authentication app started on 127.0.0.1:3030');
```
>>>>>>> adding example

## Changelog

__0.1.0__

- Initial release

## License

Copyright (c) 2015

Licensed under the [MIT license](LICENSE).
