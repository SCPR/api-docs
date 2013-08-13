## Categories ##
**Endpoint**: `/categories/`

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

**Endpoint**: `/categories/{slug}` (GET)  
**Params**: 
* `slug` - (String) The slug (uuid) for the category.

**Example** GET `/categories/film`  
**Returns** A single JSON object representation of the requested category.

#### Category Collection ####
Get all categories.

**Endpoint**: `/categories` (GET)  
**Params**: None  
**Example** GET `/categories`  
**Returns** A JSON array of all categories.
