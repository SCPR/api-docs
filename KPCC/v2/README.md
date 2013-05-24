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

**Current Version**: 2.3.1  
**Endpoint**: /api/v2/  
**Response**: JSON only


## Content ##
**Endpoint**: /api/v2/content/


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
    <td>(DateTime) The original publish date of the content.</td>
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
    <td>audio</td>
    <td>
      (Array) This article's Audio. See <a href="#audio-object-description">Audio Object Description</a> for the object description. <br />
      <strong>Note:</strong> This attribute will be empty if the object MAY have attributions, but has none. It will be absent if the article MAY NOT have attributions (eg. ContentShells).
    </td>
  </tr>

  <tr>
    <td>attributions</td>
    <td>
      (Array) Attributions (i.e., Bylines). See <a href="#attribution-object-description">Attribution Object Description</a> for the object description. <br />
      <strong>Note:</strong> This attribute will be empty (empty array) if the object MAY have attributions, but has none. It will be <em>absent</em> if the article MAY NOT have attributions (eg. ShowEpisodes).
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
    <td><strong>url</strong></td>
    <td>(String) The canonical URL for this category.</td>
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
  <tr><td><strong>thumbnail, small,<br />large, full</strong></td><td>(Object) Asset sizes:
    <ul>
      <li><strong>url</strong></li>
      <li><strong>width</strong></li>
      <li><strong>height</strong></li>
      <li>native
        <ul>
          <li><strong>class</strong> - (String) YoutubeVideo, BrightcoveVideo, VimeoVideo</li>
          <li><strong>id</strong> - (String) The native video ID.
        </ul>
      </li>
    </ul>
  </td></tr>
</table>


### Endpoints ###

#### Article by URL ####
Find an article by its URL.

**Endpoint**: /api/v2/content/by_url?url={url} (GET)  
**Params**:
* `url` - (String) The full URL of the content.

**Example**
GET /api/v2/content/by_url?url=http://www.scpr.org/blogs/politics/2013/04/16/13317/dearmayor-live-from-westchester-what-should-la-s-n/  
**Returns**
A single JSON object representation of the requested content.


#### Article by ID (obj_key) ####
Find an article by its obj_key (blogs/entry:999)

**Endpoint**: /api/v2/content/{obj_key} (GET)  
**Params**: 
* `obj_key` - (String) The object key (API id) for the article.

**Example**
GET /api/v2/content/blogs/entry:999  
**Returns**
A single JSON object representation of the requested content.


#### Content Collection ####
Find a collection of articles based on several parameters.

**Endpoint**: /api/v2/content?{optional params} (GET)  
**Params**: (All parameters are optional)
* `query` - (String) A search query.  
Example: ?query=Obama+Healthcare
* `types` - (comma-separated list) The types of articles to return.  
Example: ?types=news,blogs,segments  
See the "Supported Classes" table for the options. (default: all types)
* `limit` - (Integer) The number of articles to return.  
Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**
GET /api/v2/content?query=Obama&types=news,blogs,segments&limit=25&page=4  
**Returns**
A JSON array of article objects, ordered by **descending published_at date**.


#### Most Viewed ####
Grab the most viewed content.

**Endpoint**: /api/v2/content/most_viewed (GET)  
**Params**: None  
**Example**
GET /api/v2/content/most_viewed  
**Returns**
A JSON array of article objects.


#### Most Commented ####
Grab the most commented content.

**Endpoint**: /api/v2/content/most_commented (GET)  
**Params**: None  
**Example**
GET /api/v2/content/most_commented  
**Returns**
A JSON array of article objects.



## Audio ##
**Endpoint**: /api/v2/audio/


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
    <td><strong>content_obj_key</strong></td>
    <td>(String) The obj_key (UUID) for this audio's content object. You can make a separate request to the <a href="#content">Content API</a> to fetch the object.</td>
  </tr>
</table>


### Endpoints ###

#### Audio by ID ####
Find audio by its ID.

**Endpoint**: /api/v2/audio/{id} (GET)  
**Params**: 
* `id` - (Integer) The ID for the audio.

**Example**
GET /api/v2/audio/999  
**Returns**
A single JSON object representation of the requested audio.


#### Audio Collection ####
Find a collection of audio based on several parameters.

**Endpoint**: /api/v2/audio?{optional params} (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of audio objects to return.  
Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**
GET /api/v2/audio?limit=25&page=4  
**Returns**
A JSON array of audio objects, ordered by **descending uploaded_at date**.



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
