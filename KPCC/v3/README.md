# KPCC Public API v3 Documentation

### Notes on this Documentation ###
* In Object descriptions, **bold** denotes that a node will *always* be present, even if it's empty. Otherwise, the node will only be present if it isn't empty. There are some noted exceptions.
* Right now it's a little inconsistent what an "empty" attribute will look like - it could be an empty string, but may also be `null`. We recommend checking for both, just to be safe.
* Object types are loose mappings to the database field type, or a Javascript type. For example:
    * Integer (could be a String or a Number).
    * Text - Long string (potentially >1 lines).
    * String - One-line string.
    * Array
    * Object
* All Date/Time fields are in **ISO 8601** format, unless otherwise noted.

**Current Version**: 3.0.0  
**Endpoint**: `/api/v3/`  
**Response**: JSON only



## Articles ##
**Endpoint**: `/api/v3/articles/`

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
</table>

### Objects ###

#### Article Object Description ####
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

**Endpoint** `/api/v3/articles/by_url?url={url}` (GET)  
**Params**  
* `url` - (String) The full URL of the article.

**Example**  
GET `/api/v3/articles/by_url?url=http://www.scpr.org/blogs/politics/2013/04/16/13317/obama-and-stuff/`  
**Returns** A single JSON object representation of the requested article.

#### Article by ID (obj_key) ####
Find an article by its obj_key (`blogs/entry:999`)

**Endpoint** `/api/v3/articles/{obj_key}` (GET)  
**Params**:  
* `obj_key` - (String) The object key (API id) for the article.

**Example** GET `/api/v3/articles/blogs/entry:999`  
**Returns** A single JSON object representation of the requested article.

#### Articles Collection ####
Find a collection of articles based on several parameters.

**Endpoint**: `/api/v3/articles?{optional params}` (GET)  
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
GET `/api/v3/articles?query=Obama&types=news,blogs,segments&limit=25&page=4`  
**Returns** A JSON array of article objects, ordered by **descending published_at date**.

#### Most Viewed ####
Grab the most viewed articles.

**Endpoint**: `/api/v3/articles/most_viewed` (GET)  
**Params**: None  
**Example** GET `/api/v3/articles/most_viewed`  
**Returns** A JSON array of article objects.

#### Most Commented ####
Grab the most commented articles.

**Endpoint**: `/api/v3/articles/most_commented` (GET)  
**Params**: None  
**Example** GET `/api/v3/articles/most_commented`  
**Returns** A JSON array of article objects.



## Events ##

### Objects ###

#### Event Object Description ####
Representation of an Event in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The numerical ID for this event.</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title of the event.</td>
  </tr>

  <tr>
    <td><strong>public_url</strong></td>
    <td>(String) The full, canonical URL for this event.</td>
  </tr>

  <tr>
    <td><strong>starts_at</strong></td>
    <td>(DateTime) The start date and time for this event.</td>
  </tr>

  <tr>
    <td><strong>ends_at</strong></td>
    <td>(DateTime) The end date and time for this event. <strong>May be NULL.</strong></td>
  </tr>

  <tr>
    <td><strong>is_all_day</strong></td>
    <td>(Boolean) Whether or not this is an all-day event (no hard start/stop time).</td>
  </tr>

 <tr>
   <td><strong>teaser</strong></td>
   <td>(Text) Short teaser description for this event.</td>
 </tr>

 <tr>
   <td><strong>body</strong></td>
   <td>(Text) The full description for this event.</td>
 </tr>

 <tr>
   <td>past_tense_body</td>
   <td>(Text) A description that describes the event in the past tense. Meant to be used for displaying past events.</td>
 </tr>

 <tr>
   <td><strong>hashtag</strong></td>
   <td>(String) A social media hashtag for this event. Doesn't include the hash mark (#).</td>
 </tr>

 <tr>
   <td><strong>event_type</strong></td>
   <td>
    (String) The type of event. The possible types are:<br />
    <ul>
      <li><strong>comm</strong>: Forum: Community Engagement</li>
      <li><strong>cult</strong>: Forum: Cultural</li>
      <li><strong>hall</strong>: Forum: Town Hall</li>
      <li><strong>spon</strong>: Sponsored</li>
      <li><strong>pick</strong>: Staff Picks</li>
    </ul>
   </td>
 </tr>

 <tr>
   <td><strong>is_kpcc_event</strong></td>
   <td>(Boolean) Whether or not this event is sponsored by or on behalf of KPCC.</td>
 </tr>

 <tr>
   <td><strong>location</strong></td>
   <td>(Object) The location of this event. See <a href="#location-object-description">Location Object Description</a> for more.</td>
 </tr>

 <tr>
   <td><strong>sponsor</strong></td>
   <td>(Object) The sponsor for this event. See <a href="#sponsor-object-description">Sponsor Object Description</a> for more.</td>
 </tr>

 <tr>
   <td>program</td>
   <td>(Object) The associated KPCC program for this event. See <a href="#program-object-description">Program Object Description</a> for more.</td>
 </tr>

 <tr>
   <td><strong>assets</strong></td>
   <td>
     (Array) The events's assets. See <a href="#asset-object-description">Asset Object Description</a> for the object description.
   </td>
 </tr>

 <tr>
   <td><strong>audio</strong></td>
   <td>
     (Array) This events's Audio. See <a href="#audio-object-description">Audio Object Description</a> for the object description.
   </td>
 </tr>
</table>

#### Location Object Description ####

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) Location title.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The URL for more information about this location.</td>
  </tr>

  <tr>
    <td><strong>address</strong></td>
    <td>
      (Object) The address of this location:
      <ul>
        <li><strong>line_1</strong></li>
        <li><strong>line_2</strong></li>
        <li><strong>city</strong></li>
        <li><strong>state</strong></li>
        <li><strong>zip_code</strong></li>
    </td>
  </tr>
</table>

#### Sponsor Object Description ####

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) Sponsor title.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The URL for more information about this sponsor.</td>
  </tr>
</table>

### Endpoints ###

#### Event by ID ####
Find an event by its numerical ID.

**Endpoint**: `/api/v3/events/{id}` (GET)  
**Params**: 
* `id` - (Integer) The numerical ID for the event.

**Example** GET `/api/v3/events/999`  
**Returns** A single JSON object representation of the requested event.

#### Events Collection ####
Get a list of Events based on some parameters.

**All event dates are expressed and should be requested in Pacific Time.**

The `start_date` and `end_date` parameters should be in ISO date format (`YYYY-MM-DD`), and will return a 400 Bad Request if the date is malformed. You can use `start_date` and `end_date` together in order to limit the results to a range of dates.

Note that `start_date` and `end_date` both act on the `starts_at` attribute of the Events. These parameters are specifying a range of dates between which the `starts_at` attribute should fall. There is currently no way to query for a range of end dates. This does present a minor problem, in that events which started a long time ago but are still occurring (such as a daily gathering for a month-long service) may not show up on the first page of results.

Here is the behavior of requesting date ranges. Note that the number of returned results will be limited by the `limit` parameter (or the default if none is specified):

* If `start_date` and `end_date` are both specified, you get exactly what you asked for.
* If only `start_date` is specified, then you get a range of events from `start_date` to the end of time.
* If only `end_date` is specified and it's a date in the *future*, then you will get a range of events from `now` until `end_date`.
* If only `end_date` is specified and it's a date in the *past*, then you will get a range of events from the beginning of time to `end_date`.

**Endpoint**: `/api/v3/events` (GET)  
**Params**:
* `start_date` - (Date) Limit the results to only events after this date. (default: Now)
  Example: `?start_date=2013-06-13`  
* `end_date` - (Date) Limit the results to only events before this date. (default: None; i.e., the end of time)
  Example: `?start_date=2013-06-13&end_date=`  
* `types` - (comma-separated list) Limit the events to only those of these types. (default: None, i.e. all types)
  See <a href="#event-object-description">Event Object Description</a> `event_type` for the available types.
  Example: `?types=comm,cult,hall`
* `only_kpcc_events` - (Boolean) Limit the results to only KPCC-sponsored events. 
  Options are "true" or "false". (default: false)  
  Example: `?only_kpcc_events=true`

**Example**  
GET `/api/v3/events?start_date=2013-06-13&end_date=2013-06-14&types=comm`  
**Returns** A JSON array of events.



## Categories ##
**Endpoint**: `/api/v3/categories/`

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
    <td><strong>public_url</strong></td>
    <td>(String) The canonical URL.</td>
  </tr>
</table>

### Endpoints ###

#### Category by Slug (uuid) ####
Find a category by its slug (uuid).

**Endpoint**: `/api/v3/categories/{slug}` (GET)  
**Params**: 
* `slug` - (String) The slug (uuid) for the category.

**Example** GET `/api/v3/categories/film`  
**Returns** A single JSON object representation of the requested category.

#### Category Collection ####
Get all categories.

**Endpoint**: `/api/v3/categories` (GET)  
**Params**: None  
**Example** GET `/api/v3/categories`  
**Returns** A JSON array of all categories.



## Episodes ##
**Endpoint**: `/api/v3/episodes/`

### Objects ###

#### Episode Object Description ####
Representation of an Episode in the JSON response.

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>teaser</strong></td>
    <td>(Text) A short teaser/description.</td>
  </tr>

  <tr>
    <td><strong>air_date</strong></td>
    <td>(Date) The air date. Format: YYYY-MM-DD</td>
  </tr>

  <tr>
    <td><strong>public_url</strong></td>
    <td>(String) The canonical URL.</td>
  </tr>

  <tr>
    <td><strong>assets</strong></td>
    <td>
      (Array) The episode's assets. See <a href="#asset-object-description">Asset Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>audio</strong></td>
    <td>
      (Array) This episode's Audio. See <a href="#audio-object-description">Audio Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>program</strong></td>
    <td>(Object) The program on which this episode aired. See <a href="#program-object-description">Program Object Description</a> for the object description.
    </td>
  </tr>

  <tr>
    <td><strong>segments</strong></td>
    <td>(Array) The segments for this episode, represented as Articles. See <a href="#article-object-description">Article Object Description</a> for the object description.
    </td>
  </tr>
</table>

### Endpoints ###

#### Episode by ID ####
Find an episode by its id.

**Endpoint**: `/api/v3/episodes/{id}` (GET)  
**Params**: 
* `id` - (Integer) The numerical ID.

**Example** GET `/api/v3/programs/999`  
**Returns** A single JSON object representation of the requested episode.

#### Episode Collection ####
Get a list of episodes based on some parameters.

**Endpoint**: `/api/v3/episodes` (GET)  
**Params**:
* `program` - (String) The slug of the program by which to filter the episodes.
  (default: none)  
  If you pass in this parameter and the program isn't found, a 404 will be returned.  
  Example: `?program=airtalk`  
* `air_date` - (Date) Limit the episodes returned to only this date. (default: none)  
  Example: `?air_date=2013-06-25`  
* `limit` - (Integer) The number of episodes to return.  
  Maximum is 8. (default: 4)
* `page` - (Integer) The page of results to return. (default: 1)

**Example** GET `/api/v3/episodes?program=airtalk&date=2013-06-25`  
**Returns** A JSON array of the requested episodes ordered by **descending air_date**.



## Programs ##
**Endpoint**: `/api/v3/programs/`

### Objects ###

#### Program Object Description ####
Representation of a Program in the JSON response.

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>slug</strong></td>
    <td>(String) The URL slug (also acting UUID).</td>
  </tr>

  <tr>
    <td><strong>host</strong></td>
    <td>(String) The name(s) of the host(s).</td>
  </tr>

  <tr>
    <td><strong>air_status</strong></td>
    <td>
      (String) The current air status. Possible values are:<br />
      <ul>
        <li>"onair"   - Currently Airing.</li>
        <li>"online"  - Online Only (Podcast).</li>
        <li>"archive" - No longer airing, but still publicly accessible.</li>
        <li>"hidden"  - Not available or accessible.</li>
      </ul>
    </td>
  </tr>

  <tr>
    <td>twitter_handle</td>
    <td>(String) The twitter handle. Does not include the @ symbol.</td>
  </tr>

  <tr>
    <td><strong>airtime</strong></td>
    <td>(String) The human-friendly airtime. This cannot be parsed into an actual date/time object.</td>
  </tr>

  <tr>
    <td><strong>description</strong></td>
    <td>(Text) A description of this program.</td>
  </tr>

  <tr>
    <td>podcast_url</td>
    <td>(String) The URL to the podcast feed.</td>
  </tr>

  <tr>
    <td>rss_url</td>
    <td>(String) The URL to the RSS feed.</td>
  </tr>

  <tr>
    <td><strong>public_url</strong></td>
    <td>(String) The canonical URL.</td>
  </tr>
</table>

### Endpoints ###

#### Program by Slug (uuid) ####
Find a program by its slug (uuid).

**Endpoint**: `/api/v3/programs/{slug}` (GET)  
**Params**: 
* `slug` - (String) The slug (uuid).

**Example** GET `/api/v3/programs/airtalk`  
**Returns** A single JSON object representation of the requested program.

#### Program Collection ####
Get all programs.

**Endpoint**: `/api/v3/programs` (GET)  
**Params**: None  
**Example** GET `/api/v3/programs`  
**Returns** A JSON array of all programs.



## Blogs ##
**Endpoint**: `/api/v3/blogs/`

### Objects ###

#### Blog Object Description ####
Representation of a Blog in the JSON response.

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>slug</strong></td>
    <td>(String) The URL slug (also acting UUID).</td>
  </tr>

  <tr>
    <td><strong>tagline</strong></td>
    <td>(String) A short tagline.</td>
  </tr>

  <tr>
    <td><strong>description</strong></td>
    <td>(Text) A longer description of this blog.</td>
  </tr>

  <tr>
    <td>rss_url</td>
    <td>(String) The URL to the RSS feed.</td>
  </tr>

  <tr>
    <td><strong>public_url</strong></td>
    <td>(String) The canonical URL.</td>
  </tr>
</table>

### Endpoints ###

#### Blog by Slug (uuid) ####
Find a blog by its slug (uuid).

**Endpoint**: `/api/v3/blogs/{slug}` (GET)  
**Params**: 
* `slug` - (String) The slug (uuid).

**Example** GET `/api/v3/blogs/politics`  
**Returns** A single JSON object representation of the requested blog.

#### Blog Collection ####
Get all blogs.

**Endpoint**: `/api/v3/blogs` (GET)  
**Params**: None  
**Example** GET `/api/v3/blogs`  
**Returns** A JSON array of all blogs.



## Editions ##
**Endpoint**: `/api/v3/editions/`

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

**Endpoint**: `/api/v3/editions/{id}` (GET)  
**Params**: 
* `id` - (Integer) The ID for the edition.

**Example** GET `/api/v3/editions/999`  
**Returns** A single JSON object representation of the requested edition.

#### Editions Collection ####
Find a collection of editions, based on several parameters.

**Endpoint**: `/api/v3/editions?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of editions to return.  
  Maximum is 4. (default: 2)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**  
GET `/api/v3/editions?limit=1` (this example will retrieve the most recent, i.e. "current", edition)  
**Returns** A JSON array of edition objects, ordered by **descending published_at date**.



## Audio ##
**Endpoint**: `/api/v3/audio/`

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
</table>

### Endpoints ###

#### Audio by ID ####
Find audio by its ID.

**Endpoint**: `/api/v3/audio/{id}` (GET)  
**Params**: 
* `id` - (Integer) The ID for the audio.

**Example** GET `/api/v3/audio/999`  
**Returns** A single JSON object representation of the requested audio.


#### Audio Collection ####
Find a collection of audio based on several parameters.

**Endpoint**: `/api/v3/audio?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of audio objects to return.  
  Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example** GET `/api/v3/audio?limit=25&page=4`  
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
