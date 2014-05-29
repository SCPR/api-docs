## v3 Changelog

### 3.0.0 (unreleased)
#### Additions
* Added a `meta` node to **every** response, which includes some metadata about the API response.
* Added `air_status` filtering to Program endpoint.
* Added a Data Points endpoint.
* Added a Tags endpoint
* Added tags node to Articles.
* Added `id` to Asset objects.

#### Changes
* **BREAKING** Response objects are now nested under an appropriately-named key.
  For example, a request to `/api/v3/articles` might return something like:

```
{
    "meta": {
        "version": "3.0.0",
        "status": {
            "code": 200,
            "message": "OK"
        }
    },

    "articles": [
        {
            "id": "blogs/entry:999",
            ...
        },
        {
            "id": "news/story:123",
            ...
        }
    ]
}
```

**version** is the current API version.  
The objects for a COLLECTION are nested under a pluralized object name as the key.
For SINGULAR resources, the key will be the SINGULAR version, such as
`article`. There are some exceptions, depending on pluralization rules (for example, "audio" is the same for plural and singular).
* Events now default to 20 per page (down from 40 from v2). This can be increased to 40.

#### Deprecations
* None

#### Removals
* Removed the `content` endpoint - use the `articles` endpoint.
* Removed `article.permalink` - use `article.public_url`
* Removed `audio.content_obj_key` - use `audio.article_obj_key`
* Removed `category.url` - use `category.public_url`
* Removed `episode.teaser`, replaced with `episode.summary`.
