## Schedule ##
**Endpoint**: `/schedule/`

### Objects ###

#### Schedule Occurrence Object Description ####
Representation of a Schedule Occurrence in the JSON response.

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>public_url</strong></td>
    <td>(String) The URL for more information.</td>
  </tr>

  <tr>
    <td><strong>starts_at</strong></td>
    <td>(DateTime) The start date/time.</td>
  </tr>
  <tr>
    <td><strong>soft_starts_at</strong></td>
    <td>(DateTime) A suggested seek destination to use as the program's start time for seeking. Intended to jump straight into program audio.</td>
  <tr>
    <td><strong>ends_at</strong></td>
    <td>(DateTime) The end date/time.</td>
  </tr>

  <tr>
    <td><strong>is_recurring</strong></td>
    <td>(Boolean) Whether or not this occurrence is part of a recurring series, or a distinct occurrence.</td>
  </tr>

  <tr>
    <td>program</td>
    <td>(Program Object) The associated program. See Program Object Description for details.</td>
  </tr>
</table>

### Endpoints ###

#### Block of Schedule ####
Get a block of schedule occurrences.

**Endpoint**: `/schedule` (GET)  
**Params**:
* `start_time` - (Integer) The **Unix Timestamp** for the date/time to 
  start the block.  
  Maximum is 1 month from now. **Requesting further than 1 month in the future will return a 400 bad request.**  
  (default: Beginning of this week, MONDAY).
* `length` - (Integer) The number of seconds of schedule to retrieve.  
  Maximum is 1 week. (default: 1 week).

**Example** GET `/schedule?start_time=1373406857&length=3600`  
**Returns** An array of schedule occurrence objects for the requested range.

#### Schedule Occurrence by Time ####
Get the schedule occurrence on at a given time.

**Endpoint**: `/schedule/at` (GET)  
**Params**:
* `time` - (Integer) The **Unix Timestamp** for the date/time to check.  
  Maximum is 1 month from now. **Requesting further than 1 month in the future will return a 400 bad request.**  
  (default: Now)

**Example** GET `/schedule/at?time=1373406857`  
**Returns** A single JSON object representation of a schedule occurrence at the requested time. Please note that **the response may be an empty object**. It's unlikely but technically possible.

#### Currently Airing ####
Get the currently airing schedule occurrence.  
**This is just a vanity path for `/schedule/at` with no params.**

**Endpoint**: `/schedule/current` (GET)  
**Params**: None  
**Example** GET `/schedule/current`  
**Returns** A single JSON object representation of a schedule occurrence at the requested time. See above for warning about empty object.
