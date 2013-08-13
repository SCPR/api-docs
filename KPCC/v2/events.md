## Events ##

### Objects ###

#### Event Object Description ####
Representation of an Event in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The numerical ID for this event.</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title of the event.</td>
  </tr>

  <tr>
    <td><strong>public_url</strong></td>
    <td>(String) The full, canonical URL for this event.</td>
  </tr>

  <tr>
    <td><strong>starts_at</strong></td>
    <td>(DateTime) The start date and time for this event.</td>
  </tr>

  <tr>
    <td><strong>ends_at</strong></td>
    <td>(DateTime) The end date and time for this event. <strong>May be NULL.</strong></td>
  </tr>

  <tr>
    <td><strong>is_all_day</strong></td>
    <td>(Boolean) Whether or not this is an all-day event (no hard start/stop time).</td>
  </tr>

 <tr>
   <td><strong>teaser</strong></td>
   <td>(Text) Short teaser description for this event.</td>
 </tr>

 <tr>
   <td><strong>body</strong></td>
   <td>(Text) The full description for this event.</td>
 </tr>

 <tr>
   <td>past_tense_body</td>
   <td>(Text) A description that describes the event in the past tense. Meant to be used for displaying past events.</td>
 </tr>

 <tr>
   <td><strong>hashtag</strong></td>
   <td>(String) A social media hashtag for this event. Doesn't include the hash mark (#).</td>
 </tr>

 <tr>
   <td><strong>event_type</strong></td>
   <td>
    (String) The type of event. The possible types are:<br />
    <ul>
      <li><strong>comm</strong>: Forum: Community Engagement</li>
      <li><strong>cult</strong>: Forum: Cultural</li>
      <li><strong>hall</strong>: Forum: Town Hall</li>
      <li><strong>spon</strong>: Sponsored</li>
      <li><strong>pick</strong>: Staff Picks</li>
    </ul>
   </td>
 </tr>

 <tr>
   <td><strong>is_kpcc_event</strong></td>
   <td>(Boolean) Whether or not this event is sponsored by or on behalf of KPCC.</td>
 </tr>

 <tr>
   <td><strong>location</strong></td>
   <td>(Object) The location of this event. See Location Object Description for more.</td>
 </tr>

 <tr>
   <td><strong>sponsor</strong></td>
   <td>(Object) The sponsor for this event. See Sponsor Object Description for more.</td>
 </tr>

 <tr>
   <td>program</td>
   <td>(Program Object) The associated KPCC program for this event. See Program Object Description for more.</td>
 </tr>

 <tr>
   <td><strong>assets</strong></td>
   <td>
     (Array of Asset Objects) The events's assets. See Asset Object Description for details.
   </td>
 </tr>

 <tr>
   <td><strong>audio</strong></td>
   <td>
     (Array of Audio Objects) This events's Audio. See Audio Object Description for details.
   </td>
 </tr>
</table>

#### Location Object Description ####

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) Location title.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The URL for more information about this location.</td>
  </tr>

  <tr>
    <td><strong>address</strong></td>
    <td>
      (Object) The address of this location:
      <ul>
        <li><strong>line_1</strong></li>
        <li><strong>line_2</strong></li>
        <li><strong>city</strong></li>
        <li><strong>state</strong></li>
        <li><strong>zip_code</strong></li>
    </td>
  </tr>
</table>

#### Sponsor Object Description ####

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) Sponsor title.</td>
  </tr>

  <tr>
    <td><strong>url</strong></td>
    <td>(String) The URL for more information about this sponsor.</td>
  </tr>
</table>

### Endpoints ###

#### Event by ID ####
Find an event by its numerical ID.

**Endpoint**: `/events/{id}` (GET)  
**Params**: 
* `id` - (Integer) The numerical ID for the event.

**Example** GET `/events/999`  
**Returns** A single JSON object representation of the requested event.

#### Events Collection ####
Get a list of Events based on some parameters.

**All event dates are expressed and should be requested in Pacific Time.**

The `start_date` and `end_date` parameters should be in ISO date format (`YYYY-MM-DD`), and will return a 400 Bad Request if the date is malformed. You can use `start_date` and `end_date` together in order to limit the results to a range of dates.

Note that `start_date` and `end_date` both act on the `starts_at` attribute of the Events. These parameters are specifying a range of dates between which the `starts_at` attribute should fall. There is currently no way to query for a range of end dates. This does present a minor problem, in that events which started a long time ago but are still occurring (such as a daily gathering for a month-long service) may not show up on the first page of results.

Here is the behavior of requesting date ranges. Note that the number of returned results will be limited by the `limit` parameter (or the default if none is specified):

* If `start_date` and `end_date` are both specified, you get exactly what you asked for.
* If only `start_date` is specified, then you get a range of events from `start_date` to the end of time.
* If only `end_date` is specified and it's a date in the *future*, then you will get a range of events from `now` until `end_date`.
* If only `end_date` is specified and it's a date in the *past*, then you will get a range of events from the beginning of time to `end_date`.

**Endpoint**: `/events` (GET)  
**Params**:
* `start_date` - (Date) Limit the results to only events after this date. (default: Now)
  Example: `?start_date=2013-06-13`  
* `end_date` - (Date) Limit the results to only events before this date. (default: None; i.e., the end of time)
  Example: `?start_date=2013-06-13&end_date=`  
* `types` - (comma-separated list) Limit the events to only those of these types. (default: None, i.e. all types)
  See Event Object Description `event_type` for the available types.
  Example: `?types=comm,cult,hall`
* `only_kpcc_events` - (Boolean) Limit the results to only KPCC-sponsored events. 
  Options are "true" or "false". (default: false)  
  Example: `?only_kpcc_events=true`

**Example**  
GET `/events?start_date=2013-06-13&end_date=2013-06-14&types=comm`  
**Returns** A JSON array of events.
