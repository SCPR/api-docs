## Tags ##
**Endpoint**: `/tags/`

### Objects ###

#### Tag Object Description ####
Representation of a Tag in the JSON response.

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>slug</strong></td>
    <td>(String) The UUID.</td>
  </tr>
</table>

### Endpoints ###

#### Tag by Slug (uuid) ####
Find a tag by its slug (uuid).

**Endpoint**: `/tags/{slug}` (GET)  
**Params**: 
* `slug` - (String) The UUID.

**Example** GET `/tags/elections-2014`  
**Returns** A single JSON object representation of the requested tag.

#### Tag Collection ####
Get all tags.

**Endpoint**: `/tags` (GET)  
**Params**: None  
**Example** GET `/tags`  
**Returns** A JSON array of all tags.
