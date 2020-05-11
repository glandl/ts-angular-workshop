# RESTful Web APIs

Introduction to [REST](https://en.wikipedia.org/wiki/Representational_state_transfer)ful Web APIs


<!-- .slide: class="left" -->
## What is *REST*?

* [*Representational state transfer*](https://en.wikipedia.org/wiki/Representational_state_transfer):
  * Originally for accessing and manipulating textual representations of Web resources using a set of stateless operations
  * Today: More generic, encompassing every entity that can be identified, named, addressed or handled, in any way whatsoever, on the Web
* Architectural pattern, not a standard
  * [Request-response](https://en.wikipedia.org/wiki/Request%E2%80%93response) pattern
* Today, [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)-based RESTful APIs dominate
  * [URLs](https://en.wikipedia.org/wiki/URL) for addressing
  * [JSON](https://en.wikipedia.org/wiki/JSON), sometimes XML for representing data elements
  * Standard [HTTP methods](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) aka *verbs* (e.g. *GET*, *PUT*, *POST*, and *DELETE*)
  * Standard [HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) for representing results
  * [HTTP header fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields) ([standard](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields) or custom) for sending parameters
  * [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) for encrypting [data in-transit](https://en.wikipedia.org/wiki/Data_in_transit)


<!-- .slide: class="left" -->
## Important Tools (Examples)

* API Clients
  * [*Postman*](https://www.getpostman.com/)
  * [*Insomnia*](https://insomnia.rest/)
  * [*REST Client*](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) in *Visual Studio Code*
* Web Debugger
  * [*Fiddler*](http://www.telerik.com/fiddler)
* HTTP Request and Response Service
  * [*httpbin.org*](https://httpbin.org)


<!-- .slide: class="left" -->
## Sample Requests

```
GET https://pokeapi.co/api/v2/pokemon HTTP/1.1
Accept: application/json

###

GET https://pokeapi.co/api/v2/pokemon/1/ HTTP/1.1
Accept: application/json
```

```
GET http://services.odata.org/V4/Northwind/Northwind.svc/Customers HTTP/1.1
Accept: application/json

###

GET http://services.odata.org/V4/Northwind/Northwind.svc/Customers HTTP/1.1
Accept: application/atom+xml

###

GET http://services.odata.org/V4/Northwind/Northwind.svc/Customers?$filter=Country%20eq%20%27Germany%27 HTTP/1.1
Accept: application/json
```
Exercise: Try this sample with different REST clients


<!-- .slide: class="left" -->
## Sample Requests (cont.)

```
POST https://httpbin.org/post HTTP/1.1
Content-Type: application/json

{ "Foo": "Bar", "Answer": 42 }

###

DELETE https://httpbin.org/delete HTTP/1.1
```
Exercise: Try this sample with different REST clients


<!-- .slide: class="left" -->
## Important REST principles

* Stateless
  * No client context stored on the server
  * Each request is complete
* Cacheable
  * Responses explicitly indicate their cacheability
* Layered System
  * Client cannot tell if connected directly to the server (e.g. reverse proxies)
* URIs
  * Resources are identified using *Uniform Resource Identifiers* (URIs)
* Resource representation
 * *XML*, *JSON*, *Atom* - today mostly JSON


<!-- .slide: class="left" -->
## RESTful Web APIs in the Browser

* Old but still relevant: [*XMLHttpRequest*](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
* Newer, but only in modern browsers: [*fetch*](https://developer.mozilla.org/en-US/docs/Web/API/GlobalFetch)
  * [Detailed MDN documentation about *fetch*...](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch)

```
const pokemonList = document.getElementById('pokemons');

(function() {
fetch('https://pokeapi.co/api/v2/pokemon/').then(response => {
  response.json().then(pokelist => {
    let html = '';
    for (const pokemon of pokelist.results) {
      html += `<li>${pokemon.name}</li>`
    }

    pokemonList.innerHTML = html;
  });
});
})();
```


<!-- .slide: class="left" -->
## RESTful Web APIs in the Browser (cont.)

With `async/await`:

```
const pokemonList = document.getElementById('pokemons');

(async function() {
    const response = await fetch('https://pokeapi.co/api/v2/pokemon/');
    const pokelist = await response.json();

    let html = '';
    for(const pokemon of pokelist.results) {
        html += `<li>${pokemon.name}</li>`
    }

    pokemonList.innerHTML = html;
})();
```


<!-- .slide: class="left" -->
## RESTful Web APIs in  the Browser (cont.)

With [*jQuery*](http://api.jquery.com/jQuery.get/):

```
(async function() {
    const pokelist = await $.get('https://pokeapi.co/api/v2/pokemon/');

    let html = '';
    for(const pokemon of pokelist.results) {
        html += `<li>${pokemon.name}</li>`
    }

    $('#pokemons')[0].innerHTML = html;
})();
```


<!-- .slide: class="left" -->
## Building RESTful Web APIs with Node.js

* In practice, frameworks are used for that
* Here: [*Express.js*](http://expressjs.com/)
  * Larger framework, not just for RESTful Web APIs
  * Very commonly used
  * Lots of plugins
* Installation
  * `npm install express`
  * For TypeScript: `npm install @types/express --save-dev`


<!-- .slide: class="left" -->
## RESTful Web API with [*Express.js*](http://expressjs.com/)

```
// If you have problems with the following line, try:
// import express = require('express');
import * as express from 'express';

var server = express();
server.get('/api/echo/:word', (request, response) => {
    response.send({youSent: request.params.word});
});

const port = 8080;
server.listen(port, function() {
  console.log(`API is listening on port ${port}`);
});
```

```
GET http://localhost:8080/api/echo/Foo-Bar HTTP/1.1
Accept: application/json
```


<!-- .slide: class="left" -->
## RESTful Web API with [*Express.js*](http://expressjs.com/)

* `express()` function
  * Creates an Express application
  * [Documentation](https://expressjs.com/en/4x/api.html#express)
* Application
  * Represents the Express application
  * Created with `express()`
  * [Documentation](https://expressjs.com/en/4x/api.html#app)
* `request` object
  * Represents the HTTP request
  * Use it to get headers, parameters, body, etc.
  * [Documentation](https://expressjs.com/en/4x/api.html#req)
* `response` object
  * Represents the HTTP response
  * Use it to build response (e.g. status, headers, body, etc.)
  * [Documentation](https://expressjs.com/en/4x/api.html#res)


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Examples

```
// If you have problems with the following line, try:
// import express = require('express');
import * as express from 'express';

import {deleteSingle} from './delete-single';
import {getAll} from './get-all';
import {getSingle} from './get-single';
import {post} from './post';

const app = express();
app.use(express.json());

// Add routes
app.get('/api/customers', getAll);
app.post('/api/customers', post);
app.get('/api/customers/:id', getSingle);
app.delete('/api/customers/:id', deleteSingle);

app.listen(8080, () => console.log('API is listening on port 8080'));
```


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Examples (cont.)

```
export interface ICustomer {
  id: number;
  firstName: string;
  lastName: string;
}

export const customers: ICustomer[] = [
  {id: 1, firstName: 'Donald', lastName: 'Duck'},
  {id: 2, firstName: 'Mickey', lastName: 'Mouse'},
  {id: 3, firstName: 'Minnie', lastName: 'Mouse'},
  {id: 4, firstName: 'Scrooge', lastName: 'McDuck'}
];
```

```
import {Request, Response} from 'express';
import {customers} from './data';

export function getAll(req: Request, res: Response): void {
    res.send(customers);
}
```


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Examples (cont.)

```
import {Request, Response} from 'express';
import {NOT_FOUND, BAD_REQUEST} from 'http-status-codes';
import {customers} from './data';

export function getSingle(req: Request, res: Response): void {
  const id = parseInt(req.params.id);
  if (id) {
    const customer = customers.find(c => c.id === id);
    if (customer) {
      res.send(customer);
    } else {
      res.status(NOT_FOUND).send();
    }
  } else {
    res.status(BAD_REQUEST).send('Parameter id must be a number');
  }
}
```


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Examples (cont.)

```
import {CREATED, BAD_REQUEST} from 'http-status-codes';
import {Request, Response} from 'express';
import {customers, ICustomer} from './data';

export function post(req: Request, res: Response): void {
  if (!req.body.id || !req.body.firstName || !req.body.lastName) {
    res.status(BAD_REQUEST).send('Missing mandatory member(s)');
  } else {
    const newCustomerId = parseInt(req.body.id);
    if (!newCustomerId) {
      res.status(BAD_REQUEST).send('ID has to be a numeric value');
    } else {
      const newCustomer: ICustomer = { id: newCustomerId,
        firstName: req.body.firstName, lastName: req.body.lastName };
      customers.push(newCustomer);
      res.status(CREATED).header({Location: `${req.path}/${req.body.id}`}).send(newCustomer);
    }
  }
}
```


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Examples (cont.)

```
import {NO_CONTENT, NOT_FOUND, BAD_REQUEST} from 'http-status-codes';
import {Request, Response} from 'express';
import {customers} from './data';

export function deleteSingle(req: Request, res: Response): void {
  const id = parseInt(req.params.id);
  if (id) {
    const customerIndex = customers.findIndex(c => c.id === id);
    if (customerIndex !== (-1)) {
      customers.splice(customerIndex, 1);
      res.status(NO_CONTENT).send();
    } else {
      res.status(NOT_FOUND).send();
    }
  } else {
    res.status(BAD_REQUEST).send('Parameter id must be a number');
  }
}
```


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Server-Side Rendering

* Express.js also supports *server-side rendering*
* HTML is generated on the server-side
  * Many different [template engines](https://github.com/expressjs/express/wiki#template-engines) are supported
  * Here we use [handlebars](http://handlebarsjs.com/)
* Server-Side Rendering can be combined with client-side logic


<!-- .slide: class="left" -->
## Server-side rendering

<img src="images/node-server-side.svg" width="125%" />


<!-- .slide: class="left" -->
## Server-side rendering

* Client is a browser
  * Explicitly started by user
  * Embedded browser (e.g. [CEFSharp](https://github.com/cefsharp/CefSharp), [WebView](https://developer.chrome.com/multidevice/webview/gettingstarted))
* Most of the business logic runs on the server
  * Minor parts of the logic runs on the client<br/>
    (e.g. form validation in JavaScript)
  * Server accesses databases and external services
* Server generates HTML, CSS, JavaScript


<!-- .slide: class="left" -->
## Web APIs + Single Page Apps (SPA)

<img src="images/node-spa.svg" width="125%" />


<!-- .slide: class="left" -->
## Web APIs + Single Page Apps (SPA)

* Client can be a browser
  * Anything that can speak HTTP, JSON, etc.<br/>
    (e.g. mobile app, CLI, server, desktop app, IoT device)
* Static HTML/CSS/JS for SPA
* Logic
  * HTTP Web API requests for running server-side business logic
  * View logic (e.g. manipulating DOM) runs on client
  * [JSON](https://rstropek.github.io/ts-angular-workshop/#/3/7) for transmitting data


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Server-Side Rendering

```
import * as path from 'path';
import * as express from 'express';

const app = express();

app.set('view engine', 'hbs');
app.set('views', path.join(__dirname, '/views'));

app.get('/', function(req, res) {
  res.render('index', {
    title: 'Hey',
    message: 'Hello there!',
    todos: [ { id: 1, desc: 'Buy food' }, { id: 2, desc: 'Homework' },
      { id: 3, desc: 'Play video games' } ]
  });
});

app.listen(8080, () => console.log('API is listening on port 8080'));
```


<!-- .slide: class="left" -->
## [*Express.js*](http://expressjs.com/) Server-Side Rendering (cont.)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
</head>
<body>
    <h1>{{ message }}</h1>
    {{#if todos}}
    <ul>
        {{#each todos}}
        <li>{{desc}}</li>
        {{/each}}
    </ul>
    {{else}}
    <p>No todos</p>
    {{/if}}
</body>
</html>
```


<!-- .slide: class="left" -->
## Further Readings and Exercises

* Want to know more? Read/watch...
  * [Microsoft's REST API Guidelines](https://github.com/Microsoft/api-guidelines/blob/vNext/Guidelines.md)
  * [*Express.js* documentation](https://expressjs.com/)
* Exercises
  * [*CouchDB* exercise](https://github.com/rstropek/ts-angular-workshop/blob/master/rest-fundamentals/9010-couch/readme.md)
  * [*Pokelist* exercise](https://github.com/rstropek/ts-angular-workshop/blob/master/rest-fundamentals/9040-pokelist/readme.md)
  * [*Address Book* exercise](https://github.com/rstropek/ts-angular-workshop/blob/master/rest-fundamentals/9050-address-book/readme.md)
