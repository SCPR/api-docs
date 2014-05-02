# KPCC Public API v3 Documentation

**WARNING** v3 of the API is in BETA and shouldn't be used yet.

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
* `version` - the API version, for reference.
* An object containing the single requested object, or an array of objects.
For example, if you requested `/articles/most_viewed`, the response will look
something like:

```json
  {
    "version": "3.0.0",
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
* [Data Points](endpoints/articles.md#data_points)



## Errors ##
These are some errors you might come across when interacting with the API.

<table>
  <tr>
    <th>Error Name</th>
    <th>Status</th>
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
