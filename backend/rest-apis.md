# REST API Basics

**Resource**
  * A model in an REST application
  * Piece of data
  * Ex: Player score, shopping cart, bank account, etc
  * Resources are things that clients of the API will want to **C**reate, **R**etreive, **Update**, **D**elete

**Endpoints**
  * The specific URLs where resources are CRUD-ed
  * represents a single or collection of records
  * Ex: `/api/v1/games` (collection of games), `/api/v1/game/1234` (single game)

**HTTP Methods**  
  * Actions that you take on resources
  * The HTTP verbs/ methods:
    * GET - fetches single/collection of resources
    * POST - adds new resource to collection 
      * POST to collection like `/api/v1/games` 
      * Don't POST to specific resource like `/api/v1/game/1234`
    * PUT - updates a record
      * not used on collections of records
    * DELETE - deletes a record
      * deleting an entire collection is a bad idea

**URL vs URI**
  * URI - Uniform resource identifier
    * Ex: `/api/v1/games/1234`
  * URL - Uniform resource locator
    * Provides full details to locate the resource
    * Ex"  `https://gamestop.com/api/v1/game/1234`

**Query strings**
  * comes after a question mark `?` in a URL
  * consists of keys and values
  * Ex: `https://gamestop.com/api/v1/games?category=ps4`, key: category, value: ps4

**HTTP HEADERS** *some headers*
  * Accept: specifies the file format the requester wants (ex: application/json)
  * Accept-Language specifies the language (ex: english)
  * to make request with JSON:
```
Content-Type: application/json
Accept: application/json
```

**URL Encoding**
  * URL encoding converts characters into a format that can be transmitted over the internet
  * Encoding type is determined by `enctype`
  * Three types of `enctype`:
    * `application/x-www-form-urlencoded` - represents a URL encoded form; default value of `enctype`
    * `multipart/form-data` - represents a multipart form which is the form used when user wants to upload files
    * `text/plain` - data without any encoding

**API versioning**
  * used to ensure legacy apis work as long as possible
  * when application updates its API, then API version is updated
    * this allows the old and new APIs to exist and not break for clients

**HTTP Status Codes**
* 100-199: Informational responses
* 200-299: Successful responses
* 300-399: Redirects
* 400-499: Client errors
* 500-599: Server errors

**Cache**
* Service that runs in memory to hold recently requested results
* it holds onto data that needs to be retrieved quickly
* Useful in making sure API is available for clients
* Useful for when data takes awhile to retrieve or calculate
* Some cache examples:
  * memcached
  * TimesTen
  * HazelCast
  * Varnish

**Rate Limiting**
* This is where each user is allowed only a certain number of requests in a given time period
* Helps prevent DDOS attacks or being flooded with requests

**API Tokens**
* Users of API are given a token and a secret pair of credentials
* Users send this pair and token to server for every request, and server uses this to authenticate users
* Sending token & keys to server
  * stored as keys in the JSON body/ XML data and then sent to the server
  * Included in the authentication headers in the HTTP request