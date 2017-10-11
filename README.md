# erpnext-node

Node library to use the frappe/erpnext api

## Installation

Add the following line under dependencies in your package.json:

```json
"erpnext-node": "sohzaz/erpnext-node"
```

Then run npm

```bash
npm install
```

## Usage

In order to use this module, first import it in you application:

```js
const ERPNext = require('erpnext-node');
```

Next, initialize the ERPNext object with the connection information for your Erpnext instance:

```js
const erp = new ERPNext({
host: 'HOST', //e.g. 'http://localhost:8000'
user: 'USER',
password: 'PASSWORD'
```

### Making Remote Procedure Calls

```js
erp.call('dotted.path.to.function', data)
    .then(res => {//do something})
    .catch(err => {//handle error})
```
The Call method accepts the frappe/erpnext method path and the necessary input data.
It returns a Promise object.


### Using Resources

Start by selecting the resource to use:

```js
const resource = erp.resource('DOCTYPE')
```

the resource object exposes those methods:

1. `find(params)`

    Returns an Array.
    params can contain any of these properties:

    | name | type |  |
    | --- | --- | --- |
    | fields | Array | An array of field names to be included |
    | filters | Array | An array of sql conditions each following this format :  `[{doctype}, {field}, {operator}, {operand}]`|
    | page_length | Number | The number of results returned per page |
    | page_start | Number | The offset at which the first result of the page should be |


2. `get(docname)`

    Returns a document as an Object

    parameters :

    | name | type |  |
    | ---- | ----- | ---- |
    | docname | String | The name of the document to fetch |


3. `create(data)`

    Persist a new document and returns it as an Object

    parameters :

    | name | type |
    | ---- | ----- |
    | data | Object |


4. `update(docname, data)`

    Update a document

    parameters :

    | name | type |
    | ---- | ----- |
    | docname | String |
    | data | Object |


5. `delete(docname)`

    Delete a document

    parameters:

    | name | type |
    | ---- | ----- |
    | docname | String |


## Exemples

  * Create a Sales Invoice from a Sales Order

  ```js
  const salesOrderName = 'SO-test';
  erp.call('erpnext.selling.doctype.sales_order.sales_order.make_sales_invoice',
        {source_name: salesOrderName})
      .then(tmpInvoice => {
            return erp.resource('Sales Invoice')
                      .create(tmpInvoice)
      })
      .then(invoice => {
        console.log(invoice)
      })
      .catch(err => {throw err})

  ```

  * Find all variants for a item template

  ```js
  erp.resource('Item')
     .find({filters: [['variant_of', '=', 'item']]})
     .then(res => {console.log(res)})
     .catch(err => {throw err})
  ```
## License (ISC)

In case you never heard about the [ISC license](http://en.wikipedia.org/wiki/ISC_license) it is functionally equivalent to the MIT license.

See the [LICENSE file](LICENSE) for details.