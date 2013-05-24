# AudioVision Public API v1 Documentation

### Notes on this Documentation ###
* In Object descriptions, **bold** denotes that a node will *always* be present, even if it's empty. Otherwise, the node will only be present if it isn't empty. There are some noted exceptions.
* Object types are loose mappings to the database field type, or a Javascript type. For example:
    * Integer (could be a String or a Number).
    * Text - Long string (potentially >1 lines).
    * String - One-line string.
    * Array
    * Object
* All Date/Time fields are in **ISO 8601** format, unless otherwise noted.

**Current Version**: 1.4.0  
**Endpoint**: /api/v1/  
**Response**: JSON only



## Posts ##
**Endpoint**: /api/v1/posts/


### Objects ###

#### Post Object Description ####
This is how every post is represented by the API in its response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(String) The object key (i.e. UUID, such as posts:999).</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The full title.</td>
  </tr>

  <tr>
    <td><strong>subtitle</strong></td>
    <td>(String) A subtitle, somewhere between "title" and "teaser".</td>
  </tr>

  <tr>
    <td><strong>byline</strong></td>
    <td>(String) The compiled, canonical byline.</td>
  </tr>

  <tr>
    <td><strong>published_at</strong></td>
    <td>(DateTime) The original publish date of the post.</td>
  </tr>

  <tr>
    <td><strong>teaser</strong></td>
    <td>(Text) A short teaser for the post.</td>
  </tr>

  <tr>
    <td><strong>body</strong></td>
    <td>(Text) The full body copy.</td>
  </tr>

  <tr>
    <td><strong>permalink</strong></td>
    <td>(String) The full, canonical URL.</td>
  </tr>

  <tr>
    <td><strong>thumbnail</strong></td>
    <td>(String) An IMG tag for the thumbnail (188x188)</td>
  </tr>

  <tr>
    <td>category</td>
    <td>
      (Object) The post's category. See <a href="#category-object-description">Category Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>assets</strong></td>
    <td>
      (Array) The post's assets. See <a href="#asset-object-description">Asset Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>attributions</strong></td>
    <td>
      (Array) Attributions (i.e., Bylines). See <a href="#attribution-object-description">Attribution Object Description</a> for the object description.
    </td>
  </tr>
</table>


#### Category Object Description ####

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The numerical ID for this category.</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The category title.</td>
  </tr>

  <tr>
    <td><strong>slug</strong></td>
    <td>(String) The URL slug for this category (text ID).</td>
  </tr>

  <tr>
    <td><strong>description</strong></td>
    <td>(String) A short description about this category.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The canonical URL for this category.</td>
  </tr>
</table>


#### Attribution Object Description ####

<table>
  <tr>
    <td><strong>name</strong></td>
    <td>(String) Name.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) A URL to more information about the name.</td>
  </tr>

  <tr>
    <td><strong>role_text</strong></td>
    <td>(String) The text description for this attribution's Role. (ex. Author, Source, or Contributor)</td>
  </tr>

  <tr>
    <td><strong>role</strong></td>
    <td>(Integer) The numeric ID for this attribution's role.</td>
  </tr>
</table>


#### Asset Object Description ####
There are four sizes of assets. These are their names and geometry (see [ImageMagick geometry](http://www.imagemagick.org/script/command-line-processing.php#geometry) for explanation). Note that `#` means "cropped".

* lsquare (188x188#)
* small (450x450>)
* large (730x486>)
* full (1024x1024>)

<table>
  <tr><td><strong>title</strong></td><td>(String) Asset title</td></tr>
  <tr><td><strong>caption</strong></td><td>(Text) Asset caption</td></tr>
  <tr><td><strong>owner</strong></td><td>(String) Asset owner</td></tr>
  <tr><td>native</td><td>(Object) The native video attributes.
    <ul>
      <li><strong>class</strong> - (String) YoutubeVideo, BrightcoveVideo, VimeoVideo</li>
      <li><strong>id</strong> - (String) The native video ID.
    </ul>
  </td></tr>
  <tr><td><strong>thumbnail, small,<br />large, full</strong></td><td>(Object) Asset sizes:
    <ul>
      <li><strong>url</strong></li>
      <li><strong>width</strong></li>
      <li><strong>height</strong></li>
    </ul>
  </td></tr>
</table>


### Endpoints ###

#### Post by URL ####
Find a post by its URL.

**Endpoint**: /api/v1/posts/by_url?url={url} (GET)  
**Params**:
* `url` - (String) The full URL of the post.

**Example**
GET /api/v1/posts/by_url?url=http://audiovision.scpr.org/127/moon-rise-los-angeles  
**Returns**
A single JSON object representation of the requested post.


#### Post by ID (obj_key) ####
Find a post by its obj_key (posts:999)

**Endpoint**: /api/v1/posts/{obj_key} (GET)  
**Params**: 
* `obj_key` - (String) The object key (UUID) for the post.

**Example**
GET /api/v1/posts/posts:999  
**Returns**
A single JSON object representation of the requested post.


#### Posts Collection ####
Find a collection of posts based on several parameters.

**Endpoint**: /api/v1/posts?{optional params} (GET)  
**Params**: (All parameters are optional)
* `query` - (String) A search query.  
Example: ?query=Obama
* `category` - (String) The slug of a category to filter by.  
Example: ?category=video  
(default: all categories)
* `limit` - (Integer) The number of articles to return.  
Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**
GET /api/v1/posts?query=Obama&category=video&limit=25&page=4  
**Returns**
A JSON array of post objects, ordered by **descending published_at date**.




## Buckets ##
Buckets are collections of posts.

**Endpoint**: /api/v1/buckets/


### Objects ###

#### Bucket Object Description ####

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(String) The text-id (Key).</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The full title.</td>
  </tr>

  <tr>
    <td><strong>Description</strong></td>
    <td>(Text) A short description.</td>
  </tr>

  <tr>
    <td><strong>updated_at</strong></td>
    <td>(DateTime) The timestamp for when this bucket was last updated.</td>
  </tr>

  <tr>
    <td>posts</td>
    <td>(Array) The posts inside of this bucket.<br /><strong>This Array is only present when requesting a single, specific Bucket</strong><br />See <a href="#post-object-description">Post Object Description</a> for more.</td>
  </tr>
</table>


### Endpoints ###

#### Bucket by ID (key) ####
Find a bucket by its key (text ID)

**Endpoint**: /api/v1/buckets/{key} (GET)  
**Params**: 
* `key` - (String) The KEY for the bucket.

**Example**
GET /api/v1/buckets/featured  
**Returns**
A single JSON object representation of the requested bucket, **including its posts**.


#### Buckets Collection ####
Finds all buckets.

**Endpoint**: /api/v1/buckets (GET)  
**Params**: None

**Example**
GET /api/v1/buckets  
**Returns**
A JSON Array of all buckets, **excluding posts**.



## Billboards ##
Featured collections of posts on the homepage of AudioVision.
**Endpoint**: /api/v1/billboards/


### Objects ###

#### Billboard Object Description ####

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(String) The object key (UUID). <em>billboards:12</em></td>
  </tr>

  <tr>
    <td><strong>published_at</strong></td>
    <td>(DateTime) The date and time that this billboard was originally published.</td>
  </tr>

  <tr>
    <td><strong>layout</strong></td>
    <td>(Integer) The layout ID. This is useless to you, you should ignore it.</td>
  </tr>

  <tr>
    <td><strong>updated_at</strong></td>
    <td>(DateTime) The timestamp for when this billboard was last updated.</td>
  </tr>

  <tr>
    <td>posts</td>
    <td>(Array) The posts inside of this billboard.<br /><strong>This Array is only present when requesting a single, specific Billboard</strong><br />See <a href="#post-object-description">Post Object Description</a> for more.</td>
  </tr>
</table>


### Endpoints ###

#### Billboard by ID ####
Find a bucket by its numerical ID.

**Endpoint**: /api/v1/billboards/{id} (GET)  
**Params**: 
* `id` - (Integer) The ID for the billboard.

**Example**
GET /api/v1/billboards/3  
**Returns**
A single JSON object representation of the requested billboard, **including its posts**.


#### Current Billboard ####
Find the billboard currently featured on AudioVision's homepage.

**Endpoint**: /api/v1/billboards/current (GET)  
**Params**: None.

**Example**
GET /api/v1/billboards/current  
**Returns**
A single JSON object representation of the requested billboard, **including its posts**.


#### Billboard Collection ####
Finds all published billboards.

**Endpoint**: /api/v1/billboard (GET)  
**Params**: None

**Example**
GET /api/v1/billboard  
**Returns**
A JSON Array of all published billboards, **excluding posts**.



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
    <td>Some parameter is malformed, such as an invalid URI in content#by_url</td>
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
