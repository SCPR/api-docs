## Audio ##
**Endpoint**: `/audio/`

### Objects ###

#### Audio Object Description ####
Representation of Audio in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The ID of this audio object. <strong>This may be NULL</strong> when receiving an audio object through an article. Position can be used as a local ID replacement in that case.</td>
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
    <td>(Integer) The duration of this audio in <strong>seconds</strong>. <strong>This may be NULL</strong>, if the duration is unknown.</td>
  </tr>

  <tr>
    <td><strong>filesize</strong></td>
    <td>(Integer) The filesize of the audio file in <strong>bytes</strong>. <strong>This may be NULL</strong>, if the size is unknown.</td>
  </tr>

  <tr>
    <td><strong>article_obj_key</strong></td>
    <td>(String or Null) The obj_key (UUID) for this audio's associated article. You can make a separate request to the <a href="#articles">Article API</a> to fetch the object. <strong>This may be NULL.</strong></td>
  </tr>
</table>

### Endpoints ###

#### Audio by ID ####
Find audio by its ID.

**Endpoint**: `/audio/{id}` (GET)  
**Params**: 
* `id` - (Integer) The ID for the audio.

**Example** GET `/audio/999`  
**Returns** A single JSON object representation of the requested audio.


#### Audio Collection ####
Find a collection of audio based on several parameters.

**Endpoint**: `/audio?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of audio objects to return.  
  Maximum is 40. (default: 10)
* `page` - (Integer) The page of results to return. (default: 1)

**Example** GET `/audio?limit=25&page=4`  
**Returns** A JSON array of audio objects, ordered by **descending uploaded_at date**.
