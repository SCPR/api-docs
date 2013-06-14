# KPCC Public API v2 Documentation

### Notes on this Documentation ###
* In Object descriptions, **bold** denotes that a node will *always* be present, even if it's empty. Otherwise, the node will only be present if it isn't empty. There are some noted exceptions.
* Object types are loose mappings to the database field type, or a Javascript type. For example:
    * Integer (could be a String or a Number).
    * Text - Long string (potentially >1 lines).
    * String - One-line string.
    * Array
    * Object
* All Date/Time fields are in **ISO 8601** format, unless otherwise noted.

**Current Version**: 2.4.0  
**Endpoint**: `/api/v2/`  
**Response**: JSON only



## Articles ##
**Endpoint**: `/api/v2/articles/`

#### Supported Classes ###
Note that NewsStory and ContentShell are lumped together.

<table>
  <tr>
    <th>Class name</th>
    <th>API type</th>
    <th>ID prefix</th>
  </tr>
  <tr>
    <td>NewsStory + ContentShell</td>
    <td>news</td>
    <td>news/story</td>
  </tr>
  <tr>
    <td>BlogEntry</td>
    <td>blogs</td>
    <td>blogs/entry</td>
  </tr>
  <tr>
    <td>ShowSegment</td>
    <td>segments</td>
    <td>shows/segment</td>
  </tr>
  <tr>
    <td>ShowEpisode</td>
    <td>episodes</td>
    <td>shows/episode</td>
  </tr>
</table>

### Objects ###

#### Content Object Description ####
This is how every article is represented by the API in its response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(String) The object key (i.e. UUID, such as blogs/entry:999).</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The full title.</td>
  </tr>

  <tr>
    <td><strong>short_title</strong></td>
    <td>(String) The short title.</td>
  </tr>

  <tr>
    <td><strong>byline</strong></td>
    <td>(String) The compiled, canonical byline.</td>
  </tr>

  <tr>
    <td><strong>published_at</strong></td>
    <td>(DateTime) The original publish date of the article.</td>
  </tr>

  <tr>
    <td><strong>teaser</strong></td>
    <td>(Text) The teaser.</td>
  </tr>

  <tr>
    <td><strong>body</strong></td>
    <td>(Text) The full body copy.</td>
  </tr>

  <tr>
    <td><strong>public_url</strong></td>
    <td>(String) The full, canonical URL.</td>
  </tr>

  <tr>
    <td><strong>permalink</strong></td>
    <td><strong>DEPRECATED</strong>: Use <em>public_url</em>.</td>
  </tr>

  <tr>
    <td><strong>thumbnail</strong></td>
    <td>(String) An IMG tag for the thumbnail (188x188)</td>
  </tr>

  <tr>
    <td>category</td>
    <td>
      (Object) The article's category. See <a href="#category-object-description">Category Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>assets</strong></td>
    <td>
      (Array) The article's assets. See <a href="#asset-object-description">Asset Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>audio</strong></td>
    <td>
      (Array) This article's Audio. See <a href="#audio-object-description">Audio Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>attributions</strong></td>
    <td>
      (Array) Attributions (i.e., Bylines). See <a href="#attribution-object-description">Attribution Object Description</a> for the object description.
    </td>
  </tr>
</table>

#### Attribution Object Description ####

<table>
  <tr>
    <td><strong>name</strong></td>
    <td>(String) Name</td>
  </tr>

  <tr>
    <td><strong>role_text</strong></td>
    <td>(String) The text description for this attribution's Role. (ex. Primary or Contributing)</td>
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

#### Article by URL ####
Find an article by its URL.

**Endpoint** `/api/v2/articles/by_url?url={url}` (GET)  
**Params**  
* `url` - (String) The full URL of the article.

**Example**  
GET `/api/v2/articles/by_url?url=http://www.scpr.org/blogs/politics/2013/04/16/13317/obama-and-stuff/`  
**Returns** A single JSON object representation of the requested article.

#### Article by ID (obj_key) ####
Find an article by its obj_key (`blogs/entry:999`)

**Endpoint** `/api/v2/articles/{obj_key}` (GET)  
**Params**:  
* `obj_key` - (String) The object key (API id) for the article.

**Example** GET `/api/v2/articles/blogs/entry:999`  
**Returns** A single JSON object representation of the requested article.

#### Articles Collection ####
Find a collection of articles based on several parameters.

**Endpoint**: `/api/v2/articles?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `query` - (String) A search query.  
  Example: `?query=Obama+Healthcare`
* `types` - (comma-separated list) The types of articles to return.  
  Example: `?types=news,blogs,segments`  
  See the [Supported Classes](#supported-classes) table for the options. (default: all types)
* `categories` - (comma-separated list) The slugs of the categories
  by which you want to filter.  
  Example: `?categories=film,music`  
  See [Categories](#categories) for how to find category slugs.
* `limit` - (Integer) The number of articles to return.  
  Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**  
GET `/api/v2/articles?query=Obama&types=news,blogs,segments&limit=25&page=4`  
**Returns** A JSON array of article objects, ordered by **descending published_at date**.

#### Most Viewed ####
Grab the most viewed articles.

**Endpoint**: `/api/v2/articles/most_viewed` (GET)  
**Params**: None  
**Example** GET `/api/v2/articles/most_viewed`  
**Returns** A JSON array of article objects.

#### Most Commented ####
Grab the most commented articles.

**Endpoint**: `/api/v2/articles/most_commented` (GET)  
**Params**: None  
**Example** GET `/api/v2/articles/most_commented`  
**Returns** A JSON array of article objects.



## Categories ##
**Endpoint**: `/api/v2/categories/`

### Objects ###

#### Category Object Description ####
Representation of a Category in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The numerical ID for this category.</td>
  </tr>

  <tr>
    <td><strong>slug</strong></td>
    <td>(String) The URL slug (also acting UUID) for this category.</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The category title.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The canonical URL for this category.</td>
  </tr>
</table>

### Endpoints ###

#### Category by Slug (uuid) ####
Find a category by its slug (uuid).

**Endpoint**: `/api/v2/categories/{slug}` (GET)  
**Params**: 
* `slug` - (String) The slug (uuid) for the category.

**Example** GET `/api/v2/categories/film`  
**Returns** A single JSON object representation of the requested category.

#### Category Collection ####
Get all categories.

**Endpoint**: `/api/v2/categories` (GET)  
**Params**: None  
**Example** GET `/api/v2/categories`  
**Returns** A JSON array of all categories.



## Editions ##
**Endpoint**: `/api/v2/editions/`

### Objects ###

#### Edition Object Description ####
Representation of an Edition in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The ID of this edition.</td>
  </tr>

  <tr>
    <td><strong>published_at</strong></td>
    <td>(DateTime) The date/time on which this Edition was published.</td>
  </tr>

  <tr>
    <td><strong>abstracts</strong></td>
    <td>
      (Array) An array of this Edition's Abstracts. See <a href="#abstract-object-description">Abstract Object Description</a> for the object description.
    </td>
  </tr>
</table>

#### Abstract Object Description ####
Representation of an Abstract in the JSON response.

<table>
  <tr>
    <td><strong>source</strong></td>
    <td>(String) The human-readble source for this abstract (eg, "NPR").</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The URL to the full article.</td>
  </tr>

  <tr>
    <td><strong>headline</strong></td>
    <td>
      (String) The local headline for this abstract.<br />
      This is not necessarily the headline of the actual article.
    </td>
  </tr>

  <tr>
    <td><strong>summary</strong></td>
    <td>(Text) A summary of the article. May contain HTML.</td>
  </tr>

  <tr>
    <td><strong>article_published_at</strong></td>
    <td>
      (DateTime) The original publish date of the full article at the
      remote source.<br />
      <strong>Note</strong>: This property may be null.
    </td>
  </tr>

  <tr>
    <td><strong>assets</strong></td>
    <td>
      (Array) The abstracts's assets. See <a href="#asset-object-description">Asset Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>audio</strong></td>
    <td>
      (Array) This abstracts's Audio. See <a href="#audio-object-description">Audio Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td>category</td>
    <td>
      (Object) The category locally assigned to this article. See <a href="#category-object-description">Category Object Description</a> for the object description.
    </td>
  </tr>

</table>

### Endpoints ###

#### Edition by ID ####
Find an edition by its ID.

**Endpoint**: `/api/v2/editions/{id}` (GET)  
**Params**: 
* `id` - (Integer) The ID for the edition.

**Example** GET `/api/v2/editions/999`  
**Returns** A single JSON object representation of the requested edition.

#### Editions Collection ####
Find a collection of editions, based on several parameters.

**Endpoint**: `/api/v2/editions?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of editions to return.  
  Maximum is 4. (default: 2)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**  
GET `/api/v2/editions?limit=1` (this example will retrieve the most recent, i.e. "current", edition)  
**Returns** A JSON array of edition objects, ordered by **descending published_at date**.



## Audio ##
**Endpoint**: `/api/v2/audio/`

### Objects ###

#### Audio Object Description ####
Representation of Audio in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The ID of this audio object.</td>
  </tr>

  <tr>
    <td><strong>description</strong></td>
    <td>(Text) A brief description of the audio.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The full, public URL to the audio file.</td>
  </tr>

  <tr>
    <td><strong>byline</strong></td>
    <td>(String) The byline for this audio.</td>
  </tr>

  <tr>
    <td><strong>uploaded_at</strong></td>
    <td>(DateTime) The date/time that this audio was uploaded.</td>
  </tr>

  <tr>
    <td><strong>position</strong></td>
    <td>(Integer) The audio's index in the list of its article's attached audio.</td>
  </tr>

  <tr>
    <td><strong>duration</strong></td>
    <td>(Integer) The duration of this audio in <strong>seconds</strong>.</td>
  </tr>

  <tr>
    <td><strong>filesize</strong></td>
    <td>(Integer) The filesize of the audio file in <strong>bytes</strong>.</td>
  </tr>

  <tr>
    <td><strong>article_obj_key</strong></td>
    <td>(String) The obj_key (UUID) for this audio's associated article. You can make a separate request to the <a href="#articles">Article API</a> to fetch the object.</td>
  </tr>

  <tr>
    <td><strong>content_obj_key</strong></td>
    <td><strong>DEPRECATED</strong>: Use <em>article_obj_key</em>.</td>
  </tr>
</table>

### Endpoints ###

#### Audio by ID ####
Find audio by its ID.

**Endpoint**: `/api/v2/audio/{id}` (GET)  
**Params**: 
* `id` - (Integer) The ID for the audio.

**Example** GET `/api/v2/audio/999`  
**Returns** A single JSON object representation of the requested audio.


#### Audio Collection ####
Find a collection of audio based on several parameters.

**Endpoint**: `/api/v2/audio?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of audio objects to return.  
  Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example** GET `/api/v2/audio?limit=25&page=4`  
**Returns** A JSON array of audio objects, ordered by **descending uploaded_at date**.



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
