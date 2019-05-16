## Lists ##
**Endpoint**: `/lists/`

### Objects ###

#### List Object Description ####
Representation of a List in the JSON response.

<table>
  <tr>
    <td><strong>id</strong></td>
    <td>(Integer) The ID.</td>
  </tr>

  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title.</td>
  </tr>

  <tr>
    <td><strong>context</strong></td>
    <td>(String) The context.</td>
  </tr>

  <tr>
    <td><strong>starts_at</strong></td>
    <td>(DateTime) The date that the List will becomes active.</td>
  </tr>

  <tr>
    <td><strong>ends_at</strong></td>
    <td>(DateTime) The date that the List will is no longer active.</td>
  </tr>

  <tr>
    <td><strong>created_at</strong></td>
    <td>(DateTime) The date that the List was created.</td>
  </tr>

  <tr>
    <td><strong>updated_at</strong></td>
    <td>(DateTime) The date that the List was last updated.</td>
  </tr>

  <tr>
    <td><strong>items</strong></td>
    <td>(Array of Article or Program Objects) The items in the list (Article and/or Program objects). May be heterogenous.</td>
  </tr>
</table>

### Endpoints ###

#### List Collection ####
Get all currently active lists.

**Endpoint**: `/lists` (GET)  
**Params**: None  
**Example** GET `/lists`  
**Returns** A JSON array of all currently active lists.  

#### Lists by Context (String) ####
Find all lists with a given context (String).

**Endpoint**: `/lists` (GET)  
**Params**: 
* `context` - (String) The context.

**Example** GET `/lists/?context=ios`  
**Returns** An array of JSON objects representing all active lists associated with the `ios` context.

