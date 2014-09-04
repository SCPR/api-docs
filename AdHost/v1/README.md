# AdHost Public API v1 Documentation

### Notes on this Documentation ###
* In Object descriptions, **bold** denotes that a node will *always* be present, even if it's empty. Otherwise, the node will only be present if it isn't empty. There are some noted exceptions.
* Object types are loose mappings to the database field type, or a Javascript type. For example:
    * Integer (could be a String or a Number).
    * Text - Long string (potentially >1 lines).
    * String - One-line string.
    * Array
    * Object
* All Date/Time fields are in **ISO 8601** format, unless otherwise noted.

**Endpoint**: /api/v1/  
**Response**: JSON only



## Visual Campaigns ##
**Endpoint**: /api/v1/visual_campaigns/

**NOTE**: All requests to the Visual Campaigns endpoint *must* have an `Origin` header set.

### Objects ###

#### Visual Campaign Object Description ####
This is how every visual campaign is represented by the API in its response.

<table>
  <tr>
    <td><strong>key</strong></td>
    <td>(String) The output key, such as `pushdown-global`.</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>starts_at</strong></td>
    <td>(DateTime) The start date for the campaign. This may be null.</td>
  </tr>

  <tr>
    <td><strong>ends_at</strong></td>
    <td>(DateTime) The end date for the campaign. This may be null.</td>
  </tr>

  <tr>
    <td><strong>domains</strong></td>
    <td>(String) A comma-separated list of allowed domains.</td>
  </tr>

  <tr>
    <td><strong>markup</strong></td>
    <td>(Text) The markup.</td>
  </tr>

  <tr>
    <td><strong>updated_at</strong></td>
    <td>(DateTime) The last update timestamp.</td>
  </tr>

  <tr>
    <td><strong>created_at</strong></td>
    <td>(DateTime) The date on which this campaign was created.</td>
  </tr>

  <tr>
    <td><strong>cookie_key</strong></td>
    <td>(String) The recommended key for the cookie. This may be null.</td>
  </tr>

  <tr>
    <td><strong>cookie_ttl_hours</strong></td>
    <td>(Integer) The recommended TTL in **hours** for this cookie.</td>
  </tr>
</table>


### Endpoints ###

#### Active Campaigns ####
Get the active campaigns.

**Endpoint**: /api/v1/visual_campaigns?{optional params} (GET)  
**Headers**:  
* `Origin` - **required**. The server controls where campaigns may be delivered.
**Params**:  
* `keys` - (String) Optional- A comma-separated list of output keys to filter by. (default: no filter)  
Example: ?keys=global,article,event

**Example**
GET /api/v1/visual_campaigns?keys=pushdown-global,article  
**Returns**
A JSON array of active visual campaign objects.


#### Single Active Campaign ####
Get a single active campaign by output key.

**Endpoint**: /api/v1/visual_campaigns/{key} (GET)  
**Headers**:  
* `Origin` - **required**. The server controls where campaigns may be delivered.
**Params**: None

**Example**
GET /api/v1/visual_campaigns?keys=pushdown-global,article  
**Returns**
A JSON array of active visual campaign objects.

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
    <td>You are not allowed to retrieve this campaign's information from the Origin domain.</td>
  </tr>
  <tr>
    <td>Bad Request</td>
    <td>400</td>
    <td>You are missing the Origin header.</td>
  </tr>
  <tr>
    <td>Not Found</td>
    <td>404</td>
    <td>No campaign was found with the requests key..</td>
  </tr>
  <tr>
    <td>Server Error</td>
    <td>500</td>
    <td>An unexpected error occurred. Please contact bricker@scpr.org .</td>
  </tr>
</table>
