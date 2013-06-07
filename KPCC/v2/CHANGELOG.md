### 2.4.0 (2013-07-07)
* Added Editions endpoint, for requesting Editions and their Abstracts.
* Added Category filtering for Content.
* Added Category endpoint.
* Added "slug" to the Category node.


### 2.3.1 (2013-05-22)
* Videos have been deprecated. You can request them but you'll just get a list of all content. **This is a breaking change**, but I'm not incrementing the major version because it will still be broken in v2.
* Added Native type to asset objects.


### 2.2.1 (2013-05-21)
* Added URL to Audio objects.


### 2.2.0 (2013-05-20)
* Added Audio API endpoint
* Added Audio array to Content object
* Updated documentation in the README
* Attributions are now *always* present, if they may be. So, an article with no attributions will still have an `attributions` property - it will just be a blank array. Article types that don't have attributions (eg. ShowEpisdes) will not have this property.


### 2.1.1 (2013-05-07)
* Cache content objects.


### 2.1.0 (2013-04-22)
* Added assets array (4 sizes: thumbnail, small, large, full), each with URL and dimensions. Assets array will always be present, even if there are no assets (blank array.)
* Added category node (id, title, and URL). Only shows up if available.
* Added Attributions array (name, role_text, role). Only shows up if available.
* Thumbnail node will now *always* be present, but may be NULL if no assets are available.


### 2.0.0
Initial release
