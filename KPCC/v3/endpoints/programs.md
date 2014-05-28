## Programs ##
**Endpoint**: `/programs/`

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
        <li><strong>onair</strong>: Currently Airing.</li>
        <li><strong>online</strong>: Online Only (Podcast).</li>
        <li><strong>archive</strong>: No longer airing, but still publicly accessible.</li>
        <li><strong>hidden</strong>: Not available or accessible.</li>
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

**Endpoint**: `/programs/{slug}` (GET)  
**Params**:
* `slug` - (String) The slug (uuid).

**Example** GET `/programs/airtalk`  
**Returns** A single JSON object representation of the requested program.

#### Program Collection ####
Get all programs.

**Endpoint**: `/programs` (GET)  
**Params**:
* `air_status` - (comma-separated list) The air statuses by which to filter the programs. Options are `onair`, `online`, `archive`, `hidden`. See the `air_status` property for the Program object for descriptions of each. (default: no filter)

**Example** GET `/programs?air_status=online,onair`  
**Returns** A JSON array of programs.
