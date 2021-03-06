# KPCC Public API v3 Documentation

### Notes on this Documentation ###
* All API URL's will look something like: `GET http://www.scpr.org/api/v3/articles`. Everything up to the actual object endpoint ("articles" in this case) has been snipped for brevity.
* In Object descriptions, **bold** denotes that a node will *always* be present, even if it's empty. Otherwise, the node will only be present if it isn't empty. There are some noted exceptions.
* Right now it's a little inconsistent what an "empty" attribute will look like - it could be an empty string, but may also be `null`. We recommend checking for both, just to be safe.
* Object types are loose mappings to the database field type, or a Javascript type. For example:
    * Integer (could be a String or a Number).
    * Text - Long string (potentially >1 lines).
    * String - One-line string.
    * Array
    * Object
* All Date/Time fields are in **ISO 8601** format, unless otherwise noted.

**Root Endpoint**: `/api/v3`  
**Response**: JSON only



## Payload ##
Every payload consists of the following nodes:
* `meta` - Metadata about the API response:
  * `version` - (String) The API version
  * `status`
    * `code` - (Integer) The HTTP response code (200, 404, 400, etc.)
    * `message` - (String) The message for the response code ("OK", "Not Found", "Bad Request", etc.)
* An object containing the single requested object, or an array of objects.
For example, if you requested `/articles/most_viewed`, the response will look
something like:

```json
{
  "meta": {
    "version": "3.0.0",
    "status": {
      "code": 200,
      "message": "OK"
    }
  },
  "articles": [ ... ]
}
```



## Table of Contents ##
* [Articles](endpoints/articles.md#articles)
* [Schedule](endpoints/schedule.md#schedule)
* [Events](endpoints/events.md#events)
* [Categories](endpoints/categories.md#categories)
* [Episodes](endpoints/episodes.md#episodes)
* [Programs](endpoints/programs.md#programs)
* [Buckets](endpoints/buckets.md#buckets)
* [Blogs](endpoints/blogs.md#blogs)
* [Editions](endpoints/editions.md#editions)
* [Audio](endpoints/audio.md#audio)
* [Alerts](endpoints/alerts.md#alerts)
* [Data Points](endpoints/data_points.md#data_points)
* [Tags](endpoints/tags.md#tags)
* [Lists](endpoints/lists.md#lists)
* [Settings](endpoints/settings.md#settings)


## Errors ##
The API may return an error for various reasons. The payload for an error looks like this:

```json
{
  "meta": {
    "version": "3.0.0",
    "status": {
      "code": 404,
      "message": "Not Found"
    }
  },

  "error": {
    "message": "Record Not Found"
  }
}
```

The `meta.status.message` property is the HTTP status message. The `error.message` status is a message for the specific problem. They may be the same.

These are some errors you might come across when interacting with the API.

<table>
  <tr>
    <th>Message</th>
    <th>Code</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Unauthorized</td>
    <td>401</td>
    <td>The API key is incorrect or missing (Private API only)</td>
  </tr>
  <tr>
    <td>Bad Request</td>
    <td>400</td>
    <td>Some parameter is malformed, such as an invalid URI in articles#by_url</td>
  </tr>
  <tr>
    <td>Not Found</td>
    <td>404</td>
    <td>The requested object can't be found.</td>
  </tr>
  <tr>
    <td>Server Error</td>
    <td>500</td>
    <td>An unexpected error occurred. Please contact bricker@scpr.org .</td>
  </tr>
</table>
