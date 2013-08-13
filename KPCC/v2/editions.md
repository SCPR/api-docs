## Editions ##
**Endpoint**: `/editions/`

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
      (Array) An array of this Edition's Abstracts. See <a href="#abstract-object-description">Abstract Object Description</a> for details.
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
      (Array) The abstracts's assets. See <a href="#asset-object-description">Asset Object Description</a> for details.
    </td>
  </tr>

  <tr>
    <td><strong>audio</strong></td>
    <td>
      (Array) This abstracts's Audio. See <a href="#audio-object-description">Audio Object Description</a> for details.
    </td>
  </tr>

  <tr>
    <td>category</td>
    <td>
      (Object) The category locally assigned to this article. See <a href="#category-object-description">Category Object Description</a> for details.
    </td>
  </tr>

</table>

### Endpoints ###

#### Edition by ID ####
Find an edition by its ID.

**Endpoint**: `/editions/{id}` (GET)  
**Params**: 
* `id` - (Integer) The ID for the edition.

**Example** GET `/editions/999`  
**Returns** A single JSON object representation of the requested edition.

#### Editions Collection ####
Find a collection of editions, based on several parameters.

**Endpoint**: `/editions?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of editions to return.  
  Maximum is 4. (default: 2)
* `page` - (Integer) The page of results to return. (default: 1)

**Example**  
GET `/editions?limit=1` (this example will retrieve the most recent, i.e. "current", edition)  
**Returns** A JSON array of edition objects, ordered by **descending published_at date**.
