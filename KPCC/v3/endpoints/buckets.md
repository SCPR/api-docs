## Buckets ##
**Endpoint**: `/buckets/`

### Objects ###

#### Bucket Object Description ####
Representation of a Bucket in the JSON response.

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
    <td><strong>updated_at</strong></td>
    <td>(DateTime) The last updated timestamp.</td>
  </tr>

  <tr>
    <td>articles</td>
    <td>
      (Array of Article Objects) The articles inside this bucket. See Article Object Description for details. This is only present for single-bucket requests. It won't be included on the INDEX endpoint.
    </td>
  </tr>
</table>

### Endpoints ###

#### Bucket by Slug (uuid) ####
Find a bucket by its slug (uuid).

**Endpoint**: `/buckets/{slug}` (GET)  
**Params**: 
* `slug` - (String) The slug (uuid).

**Example** GET `/buckets/homepage`  
**Returns** A single JSON object representation of the requested bucket.

#### Buckets Collection ####
Get all buckets. **Note** This endpoint doesn't include the articles inside each
bucket.

**Endpoint**: `/buckets` (GET)  
**Params**: None  
**Example** GET `/buckets`  
**Returns** A JSON array of all buckets, excluding their articles.
