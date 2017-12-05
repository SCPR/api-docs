## Settings ##
**Endpoint**: `/settings/`

### Objects ###

#### Settings Object Description ####
Representation of Settings in the JSON response.

<table>
  <tr>
    <td>plus_stream_url</td>
    <td>(String) A URL for the KPCC Plus live stream.</td>
  </tr>
</table>

#### Additional Settings (iOS) ####
When passed the `ios` context, additional settings iOS-specific will be provided.

Currently, only an optional `donate` object is returned.

##### Donate Settings (iOS) #####

<table>
  <tr>
    <td><strong>prompt_title</strong></td>
    <td>(String) The title used in the header of the in-app donation prompt.</td>
  </tr>
  <tr>
    <td><strong>prompt_text</strong></td>
    <td>(String) The promotional text used in the in-app donation prompt.</td>
  </tr>
  <tr>
    <td><strong>prompt_style</strong></td>
    <td>(String) The style of the in-app donation prompt (currently accepts "standard").</td>
  </tr>
  <tr>
    <td><strong>prompt_background_color</strong></td>
    <td>(String) The RGBA color for the in-app donation prompt background (in the format RRR,GGG,BBB,AAA).</td>
  </tr>
  <tr>
    <td><strong>prompt_button_color</strong></td>
    <td>(String) The RGBA color for the in-app donation prompt buttons (in the format RRR,GGG,BBB,AAA).</td>
  </tr>
  <tr>
    <td><strong>prompt_button_title_color</strong></td>
    <td>(String) The RGBA color for the in-app donation prompt button titles (in the format RRR,GGG,BBB,AAA).</td>
  </tr>
  <tr>
    <td><strong>prompt_text_color</strong></td>
    <td>(String) The RGBA color for the in-app donation prompt title (in the format RRR,GGG,BBB,AAA).</td>
  </tr>
  <tr>
    <td><strong>prompt_buttons</strong></td>
    <td>(Array) Array of in-app donation prompt button-specific settings. See below for details.</td>
  </tr>
</table>

##### Donate Button-Specific Settings (iOS) #####
An array of these objects may be returned in the `prompt_buttons` array, described above.

<table>
  <tr>
    <td><strong>action</strong></td>
    <td>(String) An action code specifying what the button should do when tapped. Supported action codes are:<br /><br />
    <ul>
      <li><strong>donate_10</strong>: Donate $10 (one-time)</li>
      <li><strong>donate_15</strong>: Donate $15 (one-time)</li>
      <li><strong>donate_30</strong>: Donate $30 (one-time)</li>
      <li><strong>donate_arbitrary</strong>: Donate an arbitrary amount (one-time)</li>
      <li><strong>donate_10_recurring</strong>: Donate $10 (recurring)</li>
      <li><strong>donate_15_recurring</strong>: Donate $15 (recurring)</li>
      <li><strong>donate_30_recurring</strong>: Donate $30 (recurring)</li>
      <li><strong>donate_arbitrary_recurring</strong>: Donate an arbitrary amount (recurring)</li>
    </ul>
  </tr>
  <tr>
    <td><strong>title</strong></td>
    <td>(String) The title of the button. This should be kept short to account for small display sizes.</td>
  </tr>
</table>

### Endpoints ###

#### Global settings ####
Get global settings.

**Endpoint**: `/settings` (GET)  
**Params**: None  
**Example** GET `/settings`  
**Returns** A single JSON object representation of global settings.

#### Global settings, including settings matching Context (String) ####
Get global settings, as well as those associated with a given context (String).

**Endpoint**: `/settings/{context}` (GET)  
**Params**: 
* `context` - (String) The context.

**Example** GET `/settings/ios`  
**Returns** A single JSON object representation of general settings, as well as those associated with the `ios` context.
