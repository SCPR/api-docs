## v2 Changelog

### 2.11.2 (unreleased)
#### Changes
* Bumped version to break cache.


### 2.11.1 (2013-09-01)
#### Additions
* Added Buckets endpoint.


### 2.11.0 (2013-08-22)
#### Changes
* Split Content Shells and News Stories apart in the articles endpoint. Content Shells can now be requested by adding `shells` to the `type` parameter for Articles.


### 2.10.3 (2013-08-22)
#### Fixes
* Fixed the episodes endpoint for KPCC Programs which use Segments as Episodes. If a program doesn't use episodes, an episode wrapper will be created for each segment.
* Added in missing byline to Content Shell.


### 2.10.2 (2013-08-13)
#### Fixes
* Ordered ExternalProgram's episodes by descending `air_date`, as specified in the documentation.


### 2.10.1 (2013-08-13)
#### Changes
* Added an actual published_at property to Alerts, for sorting. Alerts are now sorted by this attribute.


### 2.10.0 (2013-08-13)
#### Additions
* Added Alerts.


### 2.9.1 (2013-08-09)
#### Bug Fixes
* `Program.twitter_handle` was showing up even if it was empty, contrary to the
  documentation.


### 2.9.0 (2013-08-09)
#### Additions
* Added `program.twitter_handle`

#### Changes
* In the Episodes API, if you pass in a `program` parameter and the
  program isn't found, a 404 status will be returned. Previously,
  it just ignored the invalid slug and returned a list of recent episodes.

#### Deprecations
* Deprecated `episode.teaser`, replaced with `episode.summary`.


### 2.8.1 (2013-07-16)
#### Additions
* Added `air_status` to Program object.


### 2.8.0 (2013-07-09)
#### Additions
* Added the Schedule API.


### 2.7.1 (2013-06-25)
#### Changes
* Reduced episodes maximum results to 8 per page.
* Reduced episodes default limit to 4.


### 2.7.0 (2013-06-25)
#### Additions
* Added Blogs endpoint
* Added Programs endpoint
* Added Episodes endpoint

#### Changes
* Updated `program` object in Events to return a full program object. The API has not change.

#### Fixes
* Fix bug in Category index endpoint


### 2.6.0 (2013-06-20)
#### Changes
* Show Episodes are no longer available via the `articles` endpoint. An `episodes` endpoint will be added in a later version.


### 2.5.1 (2013-06-17)
##### Additions
* Added audio and assets to Events


### 2.5.0 (2013-06-17)
##### Additions
* Added the Events API endpoint.


### 2.4.0 (2013-06-14)
##### Additions
* Added Editions endpoint, for requesting Editions and their Abstracts.
* Added Category filtering for Content.
* Added Category endpoint.
* Added "slug" to the Category node.

##### Changes
* The `audio` node will now *always* be present on articles, even if empty.
* The `attributions` node will now *always* be present on articles, even if empty.

##### Deprecations
* The `/api/v2/content` URL has been deprecated, in favor of `/api/v2/articles`. `content` will be removed in API v3.
* Deprecated `permalink` on Article objects. Replaced with `public_url`.
* Deprecated `url` on Category object. Replaced with `public_url`.
* Deprecated audio's `content_obj_key`. Replaced with `article_obj_key`.


### 2.3.1 (2013-05-22)
##### Additions
* Added Native type to asset objects.

##### Deprecations
* Videos have been deprecated. You can request them but you'll just get a list of all content. **This is a breaking change**, but I'm not incrementing the major version because it will still be broken in v2.


### 2.2.1 (2013-05-21)
##### Additions
* Added URL to Audio objects.


### 2.2.0 (2013-05-20)
##### Additions
* Added Audio API endpoint
* Added Audio array to Content object

##### Changes
* Updated documentation in the README
* Attributions are now *always* present, if they may be. So, an article with no attributions will still have an `attributions` property - it will just be a blank array. Article types that don't have attributions (eg. ShowEpisdes) will not have this property.


### 2.1.1 (2013-05-07)
##### Changes
* Cache content objects.


### 2.1.0 (2013-04-22)
##### Additions
* Added assets array (4 sizes: thumbnail, small, large, full), each with URL and dimensions. Assets array will always be present, even if there are no assets (blank array.)
* Added category node (id, title, and URL). Only shows up if available.
* Added Attributions array (name, role_text, role). Only shows up if available.

##### Changes
* Thumbnail node will now *always* be present, but may be NULL if no assets are available.


### 2.0.0
Initial release
