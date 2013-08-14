## Alerts ##
**Endpoint**: `/alerts/`

### Objects ###

#### Alert Object Description ####
Representation of an Alert in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The ID.</td>
  </tr>

  <tr>
    <td><strong>headline</strong></td>
    <td>(String) The headline.</td>
  </tr>

  <tr>
    <td><strong>type</strong></td>
    <td>
      (String) The alert type. Options are:
      <ul>
        <li><strong>break</strong>: Breaking News</li>
        <li><strong>now</strong>: Happening Now (live event, car chase, etc.)</li>
        <li><strong>audio</strong>: Listen Live (streaming)</li>
      </ul>
    </td>
  </tr>

  <tr>
    <td><strong>published_at</strong></td>
    <td>(DateTime) The timestamp for when this alert was published.</td>
  </tr>

  <tr>
    <td>public_url</td>
    <td>(String) The URL to the full story.</td>
  </tr>

  <tr>
    <td>teaser</td>
    <td>(Text) A teaser for the full story.</td>
  </tr>

  <tr>
    <td><strong>mobile_notification_sent</strong></td>
    <td>(Boolean) Whether or not a mobile push notification has been sent out.</td>
  </tr>

  <tr>
    <td><strong>email_notification_sent</strong></td>
    <td>(Boolean) Whether or not an e-mail notification has been sent out.</td>
  </tr>
</table>

### Endpoints ###

#### Alert by ID ####
Find an alert by its ID.

**Endpoint**: `/alerts/{id}` (GET)  
**Params**: 
* `id` - (Integer) The ID for the alert.

**Example** GET `/alerts/999`  
**Returns** A single JSON object representation of the requested alert.


#### Alert Collection ####
Find all published Alerts.

**Endpoint**: `/alerts?{optional params}` (GET)  
**Params**: (All parameters are optional)
* `limit` - (Integer) The number of alert objects to return.  
  Maximum is 10. (default: 5)
* `page` - (Integer) The page of results to return. (default: 1)
* `type` - (String) Filter by the type of alert. See the `type` attribute for options. (default: none, i.e. no filtering)

**Example** GET `/alerts?limit=25&page=4&type=audio`  
**Returns** A JSON array of alert objects, ordered by **descending created_at date**.
