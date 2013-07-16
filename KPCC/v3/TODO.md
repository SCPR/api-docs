## v3 TODO

* Add version header to all API responses.
* API Response should always be an object:

{
  version: "3.0.0",
  articles: [
    ... etc ...
  ]
}

* Include the status code and status message in any errors.
* Add filtering by `air_status` for Programs.
