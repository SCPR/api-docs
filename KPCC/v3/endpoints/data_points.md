## Categories ##
**Endpoint**: `/data_points/`

### Objects ###

#### Data Point Object Description ####
Representation of a Data Point in the JSON response. Data Points are key/value pairs of data that can be used for any purpose. We mostly use them for election data.

<table>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) A short description of this data point.</td>
  </tr>

  <tr>
    <td><strong>group</strong></td>
    <td>(String) The group that this data point belongs to. Example: "election2014"</td>
  </tr>

  <tr>
    <td><strong>key</strong></td>
    <td>(String) The key.</td>
  </tr>

  <tr>
    <td><strong>value</strong></td>
    <td>(String) The value.</td>
  </tr>

  <tr>
    <td><strong>notes</strong></td>
    <td>(String) Any notes about this data point or its usage. These notes are meant for internal use only.</td>
  </tr>

  <tr>
    <td><strong>updated_at</strong></td>
    <td>(DateTime) The timestamp for when this data point was last updated.</td>
  </tr>
</table>

### Endpoints ###

#### Data Point by Key ####
Find a data point by its key.

**Endpoint**: `/data_points/{key}` (GET)  
**Params**:
* `key` - (String) The key for the data point.

**Example** GET `/data_points/percent_reporting`  
**Returns** A single **full** JSON object representation of the requested data point.

#### Data Point Collection ####
Get a collection of Data Points.

**Endpoint**: `/data_points` (GET)  
**Params**:
* `group` - (String) The group by which to filter the results. Default: no filter
* `response_format` (String) The response format. Options are: `simple, full`. The `full` response returns an array of full Data Point objects (as described above). The `simple` format returns just a key-value representation of the data points. Default: `full`

#### `response_format=simple`
```json
{
  "data_points": {
    "percent_reporting": "10%",
    "precincts_reporting": "5",
    "percent_candidate1": "90%",
    "percent_candidate2": "5%"
  }
}

```

#### `response_format=full`
```json
{
  "data_points": [
    // Full Data Point object
    { ... },
    { ... },
    { ... },
  ]
}
```
**Example** GET `/data_points?group=election_2014&response_format=simple`  
**Returns** A JSON array of data points.
