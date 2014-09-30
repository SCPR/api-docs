## Episodes ##
**Endpoint**: `/episodes/`

### Objects ###

#### Episode Object Description ####
Representation of an Episode in the JSON response.

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>summary</strong></td>
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
      (Array of Asset Objects) The episode's assets. See Asset Object Description for details.
    </td>
  </tr>

  <tr>
    <td><strong>audio</strong></td>
    <td>
      (Array of Audio Objects) This episode's Audio. See Audio Object Description for details. Note that this is reserved for <strong>full episode audio</strong>, which may not always be available, even if this episode has segments with audio. This is the case with episodes coming from NPR.
    </td>
  </tr>

  <tr>
    <td><strong>program</strong></td>
    <td>(Program Object) The program on which this episode aired. See Program Object Description for details.
    </td>
  </tr>

  <tr>
    <td><strong>segments</strong></td>
    <td>(Array of Article Objects) The segments for this episode, represented as Articles. See Article Object Description for details. This may be an empty array if there are no segments. In this scenario, use the full episode information.
    </td>
  </tr>
</table>

### Endpoints ###

#### Episode by ID ####
Find an episode by its id.

**Endpoint**: `/episodes/{id}` (GET)  
**Params**: 
* `id` - (Integer) The numerical ID.

**Example** GET `/programs/999`  
**Returns** A single JSON object representation of the requested episode.

#### Episode Collection ####
Get a list of episodes based on some parameters.

**Endpoint**: `/episodes` (GET)  
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

**Example** GET `/episodes?program=airtalk&date=2013-06-25`  
**Returns** A JSON array of the requested episodes ordered by **descending air_date**.
