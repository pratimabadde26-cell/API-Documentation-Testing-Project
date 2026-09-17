# API-Documentation-Testing-Project
Sample API documentation project created in postman-Json

This project includes a Postman collection for testing and exploring the API endpoints.

[View Postman Collection](https://pratimabadde26-6294249.postman.co/workspace/API-Sample-Project~468307fd-6136-481c-89b3-82a50357f9b8/collection/54401786-244765de-71f0-4a5b-a243-33a1f2c5f378?action=share&source=copy-link&creator=54401786)

API Reference Section
Grocery Store API
This API allows you to place a grocery order which will be ready for pick-up in the store.
This API is available at
https://simple-grocery-store-api.glitch.me (Base URL)
Alternative URL:
https://simple-grocery-store-api.online/ (HTTP only)
Endpoints
The navigation below follows the existing project structure and covers the documented Status, Products, Carts, Orders and API Authentication endpoints.
Status
Get API status
GET /status
Checks whether the API is working.

Status codes
Status code	Description
200 OK	The API is running as expected.

Example response
{
  "status": "UP"
}

Products
Get all products
GET /products
Returns a list of products from the inventory.
Parameters
Name	Type	In	Required	Description
category	string	query	No	Specifies the category of products. Supported examples include meat-seafood, fresh-produce, candy, bread-bakery, dairy, eggs and coffee.
results	integer	query	No	Number of results. Must be between 1 and 20. Default is 20.
available	boolean	query	No	Specifies product availability. By default, all products are displayed.

Status codes
Status code	Description
200 OK	Successful response.
400 Bad Request	The parameters provided are invalid.

Example response
[
  {
    "id": 4643,
    "category": "coffee",
    "name": "Starbucks Coffee Variety Pack, 100% Arabica",
    "inStock": true
  },
  {
    "id": 4646,
    "category": "coffee",
    "name": "Ethical Bean Medium Dark Roast, Espresso",
    "inStock": true
  }
]

Get a product
GET /products/:productId
Returns a single product from the inventory.
Parameters
Name	Type	In	Required	Description
productId	integer	path	Yes	Specifies the product id to retrieve.
product-label	boolean	query	No	Returns the product label in PDF format.

Status codes
Status code	Description
200 OK	Successful response.
404 Not Found	There is no product with the specified id.

Example response
{
  "id": 4643,
  "category": "coffee",
  "name": "Starbucks Coffee Variety Pack, 100% Arabica",
  "manufacturer": "Starbucks",
  "price": 40.91,
  "current-stock": 14,
  "inStock": true
}

Get products by category
GET /products?category=coffee&results=5&available=true
Uses the product-list endpoint with query parameters to return products matching the selected category and filters.
Parameters
Name	Type	In	Required	Description
category	string	query	No	Product category filter, for example coffee.
results	integer	query	No	Number of results to return; 1 to 20.
available	boolean	query	No	Filter products by availability.

Status codes
Status code	Description
200 OK	Successful response.
400 Bad Request	The query parameters are invalid.

Example response
[
  {
    "id": 4641,
    "category": "coffee",
    "name": "Don Francisco Colombia Supremo Medium Roast",
    "inStock": true
  },
  {
    "id": 4643,
    "category": "coffee",
    "name": "Starbucks Coffee Variety Pack, 100% Arabica",
    "inStock": true
  },
  {
    "id": 4646,
    "category": "coffee",
    "name": "Ethical Bean Medium Dark Roast, Espresso",
    "inStock": true
  }
]

Carts
Get a cart
GET /carts/:cartId
Returns a cart.
Parameters
Name	Type	In	Required	Description
cartId	string	path	Yes	Specifies the id of the cart to retrieve.

Status codes
Status code	Description
200 OK	Successful response.
404 Not Found	There is no cart with the specified id.

Example response
{
  "id": "bx0-ycNjqIm5IvufuuZ09",
  "items": [
    {
      "productId": 4646,
      "quantity": 1
    }
  ]
}

Get cart items
GET /carts/:cartId/items
Returns the items in a cart.
Parameters
Name	Type	In	Required	Description
cartId	string	path	Yes	Specifies the id of the cart for which the items are retrieved.

Status codes
Status code	Description
200 OK	Successful response.
404 Not Found	There is no cart with the specified id.

Example response
[
  {
    "productId": 4646,
    "quantity": 1
  },
  {
    "productId": 4643,
    "quantity": 2
  }
]

Create a new cart
POST /carts
Creates a new cart and returns its id. No request parameters are accepted.
Parameters
Name	Type	In	Required	Description
—	—	—	No	No parameters are accepted for this request.

Status codes
Status code	Description
201 Created	The cart has been created successfully.

Example response
{
  "created": true,
  "cartId": "bx0-ycNjqIm5IvufuuZ09"
}

Add an item to cart
POST /carts/:cartId/items
Adds one item to an existing cart. The request body is JSON.
Parameters
Name	Type	In	Required	Description
cartId	string	path	Yes	Specifies the cart id.
productId	integer	body	Yes	Specifies the product id.
quantity	integer	body	No	Quantity. If omitted, the default is 1.

Example request body
{
  "productId": 4646,
  "quantity": 1
}

Status codes
Status code	Description
201 Created	The item has been added successfully.
400 Bad Request	The parameters provided are invalid.

Example response
{
  "created": true,
  "itemId": 123456
}

Modify an item in the cart
PATCH /carts/:cartId/items/:itemId
Modifies the quantity of an item in the cart.
Parameters
Name	Type	In	Required	Description
cartId	string	path	Yes	Specifies the cart id.
itemId	string	path	Yes	Specifies the item id.
quantity	integer	body	Yes	New quantity for the item.

Example request body
{
  "quantity": 2
}

Status codes
Status code	Description
204 No Content	The cart item was updated successfully.
400 Bad Request	The parameters are invalid or missing.
404 Not Found	The cart or item could not be found.

Example response
No response body. HTTP 204 No Content.

Replace an item in the cart
PUT /carts/:cartId/items/:itemId
Replaces the product and quantity for an existing cart item.
Parameters
Name	Type	In	Required	Description
cartId	string	path	Yes	Specifies the cart id.
itemId	string	path	Yes	Specifies the item id.
productId	integer	body	Yes	Specifies the replacement product id.
quantity	integer	body	No	Quantity.

Example request body
{
  "productId": 4643,
  "quantity": 3
}

Status codes
Status code	Description
204 No Content	The cart item was replaced successfully.
400 Bad Request	The parameters are invalid or missing.
404 Not Found	The cart or item could not be found.

Example response
No response body. HTTP 204 No Content.

Delete an item in the cart
DELETE /carts/:cartId/items/:itemId
Deletes an item from the cart.
Parameters
Name	Type	In	Required	Description
cartId	string	path	Yes	Specifies the cart id.
itemId	string	path	Yes	Specifies the item id.

Status codes
Status code	Description
204 No Content	The cart item was deleted successfully.
404 Not Found	The cart or item could not be found.

Example response
No response body. HTTP 204 No Content.

Orders
Get all orders
GET /orders
Returns all orders created by the authenticated API client.
Parameters
Name	Type	In	Required	Description
Authorization	string	header	Yes	Bearer token for the API client.

Status codes
Status code	Description
200 OK	Successful response.
401 Unauthorized	The request has not been authenticated.

Example response
[
  {
    "id": "string",
    "customerName": "John Doe",
    "comment": "My First Order",
    "createdAt": "2026-01-01T12:00:00.000Z"
  }
]

Get a single order
GET /orders/:orderId
Returns a single order belonging to the authenticated API client.
Parameters
Name	Type	In	Required	Description
Authorization	string	header	Yes	Bearer token for the API client.
orderId	string	path	Yes	The order id.
invoice	boolean	query	No	Show the PDF invoice.

Status codes
Status code	Description
200 OK	Successful response.
401 Unauthorized	The request has not been authenticated.
404 Not Found	There is no order with the specified id associated with the API client.

Example response
{
  "id": "order-123",
  "customerName": "John Doe",
  "comment": "My First Order",
  "createdAt": "2026-01-01T12:00:00.000Z"
}

Create a new order
POST /orders
Creates a new order. Once the order is successfully submitted, the cart is deleted.
Parameters
Name	Type	In	Required	Description
Authorization	string	header	Yes	Bearer token for the API client.
cartId	string	body	Yes	The cart id.
customerName	string	body	Yes	The name of the customer.
comment	string	body	No	A comment associated with the order.

Example request body
{
  "cartId": "bx0-ycNjqIm5IvufuuZ09",
  "customerName": "John Doe",
  "comment": "My First Order"
}

Status codes
Status code	Description
201 Created	The order has been created successfully.
400 Bad Request	The parameters provided are invalid.
401 Unauthorized	The request has not been authenticated.

Example response
{
  "created": true,
  "orderId": "order-123"
}

Update an order
PATCH /orders/:orderId
Updates customer or comment information for an existing order.
Parameters
Name	Type	In	Required	Description
Authorization	string	header	Yes	Bearer token for the API client.
orderId	string	path	Yes	The order id.
customerName	string	body	No	Updated customer name.
comment	string	body	No	Updated order comment.

Example request body
{
  "customerName": "Joe Doe",
  "comment": "Updated comment"
}

Status codes
Status code	Description
204 No Content	The order has been updated successfully.
400 Bad Request	The parameters provided are invalid.
401 Unauthorized	The request has not been authenticated.
404 Not Found	There is no order with the specified id associated with the API client.

Example response
No response body. HTTP 204 No Content.

Delete an order
DELETE /orders/:orderId
Deletes an order belonging to the authenticated API client.
Parameters
Name	Type	In	Required	Description
Authorization	string	header	Yes	Bearer token for the API client.
orderId	string	path	Yes	The order id.

Status codes
Status code	Description
204 No Content	The order has been deleted successfully.
400 Bad Request	The parameters provided are invalid.
401 Unauthorized	The request has not been authenticated.
404 Not Found	There is no order with the specified id associated with the API client.

Example response
No response body. HTTP 204 No Content.

API Authentication
Some endpoints require authentication. Register an API client to obtain an access token. Authenticated requests use a bearer token in the Authorization header.
Authentication header example
Authorization: Bearer YOUR_ACCESS_TOKEN

Register a new API client
POST /api-clients
Registers an API client. The request body must be JSON. The email address does not need to be real; it is not stored on the server.
Parameters
Name	Type	In	Required	Description
clientName	string	body	Yes	The name of the API client.
clientEmail	string	body	Yes	The email address of the API client.

Example request body
{
  "clientName": "Postman API Client",
  "clientEmail": "client@example.com"
}

Status codes
Status code	Description
201 Created	The API client has been registered successfully.
400 Bad Request	The parameters provided are invalid.
409 Conflict	An API client is already registered with this email address.

Example response
{
  "accessToken": "YOUR_ACCESS_TOKEN"
}

HTTP Error Responses
Error responses use an object containing an error message.
Status code	Meaning in this API
400	Bad request / invalid request data
401	Authorization is missing or invalid
404	Requested resource was not found
409	API client is already registered

Example error response

{

  "error": "The error message associated with the request"
  
}

**Best Practices**

Always use the documented base URL and HTTP method for each endpoint.
Validate required path, query, header and request-body parameters before sending a request.
Save the cartId returned by POST /carts because it is required for subsequent cart operations.
Register an API client before calling endpoints that require authorization.
Protect access tokens and do not expose them in client-side logs or public documentation.
Check HTTP status codes and handle error responses consistently.

**Endpoints**

Method	   Endpoint	                      Purpose

GET	       /status	                     Check API status

GET	       /products	                   Get all products

GET	       /products/{productId}	        Get a product

GET	       /carts/{cartId}	               Get a cart

GET	       /carts/{cartId}/items	         Get cart items

POST	     /carts	                         Create a cart

POST	     /carts/{cartId}/items	         Add item to cart

PATCH	     /carts/{cartId}/items/{itemId}  	Modify cart item

PUT	/carts/{cartId}/items/{itemId}	        Replace cart item

DELETE	/carts/{cartId}/items/{itemId}	    Delete cart item

GET	        /orders                          Get all orders

GET	        /orders/{orderId}              	Get a single order

POST	       /orders	                      Create an order

PATCH        /orders/{orderId}	            Update an order

DELETE	    /orders/{orderId}	              Delete an order

POST	      /api-clients	                Register API client




