## Blogs ##
**Endpoint**: `/blogs/`

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

**Endpoint**: `/blogs/{slug}` (GET)  
**Params**: 
* `slug` - (String) The slug (uuid).

**Example** GET `/blogs/politics`  
**Returns** A single JSON object representation of the requested blog.

#### Blog Collection ####
Get all blogs.

**Endpoint**: `/blogs` (GET)  
**Params**: None  
**Example** GET `/blogs`  
**Returns** A JSON array of all blogs.
