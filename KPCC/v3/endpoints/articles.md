## Articles ##
**Endpoint**: `/articles/`

#### Supported Classes ###

<table>
  <tr>
    <th>Class name</th>
    <th>API type</th>
    <th>ID prefix</th>
  </tr>
  <tr>
    <td>NewsStory</td>
    <td>news</td>
    <td>news_story</td>
  </tr>
  <tr>
    <td>BlogEntry</td>
    <td>blogs</td>
    <td>blog_entry</td>
  </tr>
  <tr>
    <td>ShowSegment</td>
    <td>segments</td>
    <td>show_segment</td>
  </tr>
  <tr>
    <td>ContentShell (short teasers for external content)</td>
    <td>shells</td>
    <td>content_shell</td>
  </tr>
</table>

### Objects ###

#### Article Object Description ####
This is how every article is represented by the API in its response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(String) The object key (i.e. UUID, such as blog_entry-999).</td>
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
      (Category Object) The article's category. See Category Object Description for details.
    </td>
  </tr>

  <tr>
    <td><strong>assets</strong></td>
    <td>
      (Array of Asset Objects) The article's assets. See Asset Object Description for details.
    </td>
  </tr>

  <tr>
    <td><strong>audio</strong></td>
    <td>
      (Array of Audio Objects) This article's Audio. See Audio Object Description for details.
    </td>
  </tr>

  <tr>
    <td><strong>attributions</strong></td>
    <td>
      (Array of Attribution Objects) Attributions (i.e., Bylines). See Attribution Object Description for details.
    </td>
  </tr>

  <tr>
    <td><strong>tags</strong></td>
    <td>
      (Array of Tag Objects) Tags for this article. See Tag Object Description for details.
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

**Endpoint** `/articles/by_url?url={url}` (GET)  
**Params**  
* `url` - (String) The full URL of the article.

**Example**  
GET `/articles/by_url?url=http://www.scpr.org/blogs/politics/2013/04/16/13317/obama-and-stuff/`  
**Returns** A single JSON object representation of the requested article.

#### Article by ID (obj_key) ####
Find an article by its obj_key (`blog_entry-999`)

**Endpoint** `/articles/{obj_key}` (GET)  
**Params**:  
* `obj_key` - (String) The object key (API id) for the article.

**Example** GET `/articles/blog_entry-999`  
**Returns** A single JSON object representation of the requested article.

#### Articles Collection ####
Find a collection of articles based on several parameters.

**Endpoint**: `/articles?{optional params}` (GET)  
**Params**: (All parameters are optional unless otherwise noted)
* `date`  - (Date) The publish date by which to filter results.
  This selects everything published between 12am of the requested day,
  to 11:59:59pm of the same day.  
  Format is `YYYY-MM-DD`. (default: no filter)  
  Example: `?date=2013-10-17`
* `start_date` - (Date) The start of a date range (publish date) by which
  to filter results.  
  Without an `end_date` specified, the range will be `start_date` to now.
  This parameter is required when the `end_date` parameter is present.  
  This selects everything published between 12am of the requested day,
  to 11:59:59pm of the `end_date` (or the current day if no `end_date` is
  specified).  
  Format is `YYYY-MM-DD`. (default: no filter)  
  Example: `?start_date=2013-01-01`
* `end_date` - (Date) The end date of the range explained above.
  Format is `YYYY-MM-DD`. (default: now)  
  Example: `?start_date=2013-01-01&end_date=2013-01-31`
* `query` - (String) A search query.  
  Example: `?query=Obama+Healthcare`
* `types` - (comma-separated list) The types of articles to return.  
  Example: `?types=news,blogs,segments,shells`  
  See the [Supported Classes](#supported-classes) table for the options. (default: news,blogs,segments)
* `categories` - (comma-separated list) The slugs of the categories
  by which you want to filter.  
  Example: `?categories=film,music`  
  See Categories for how to find category slugs.
* `tags` - (comma-separated list) The slugs of the tags by which to filter.  
  Example: `?tags=elections-2014,immigration`  
  See Tags for how to find tag slugs.
* `limit` - (Integer) The number of articles to return.  
  Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**  
GET `/articles?query=Obama&types=news,blogs&limit=25&page=4`  
**Returns** A JSON array of article objects, ordered by **descending published_at date**.

#### Most Viewed ####
Grab the most viewed articles.

**Endpoint**: `/articles/most_viewed` (GET)  
**Params**: None  
**Example** GET `/articles/most_viewed`  
**Returns** A JSON array of article objects.

#### Most Commented ####
Grab the most commented articles.

**Endpoint**: `/articles/most_commented` (GET)  
**Params**: None  
**Example** GET `/articles/most_commented`  
**Returns** A JSON array of article objects.
