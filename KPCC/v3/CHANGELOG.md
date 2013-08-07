## v3 Changelog

### 3.0.0 (unreleased)
#### Additions
* Added a `version` node to **every** response, indicating the API version.

#### Changes
* **BREAKING** Response objects are now nested under an appropriately-named key.
  For example, a request to `/api/v3/articles` might return something like:

```
{
    version: "3.0.0",
    articles: [
        {
            id: "blogs/entry:999",
            ...
        },
        {
            id: "news/story:123",
            ...
        }
    ]
}
```

**version** is the current API version.  
The objects for a COLLECTION are nested under a pluralized object name as the key.
For SINGULAR resources, the key will be the SINGULAR version, such as
`article`.

#### Deprecations
* None

#### Removals
* The `content` endpoint - use the `articles` endpoint.
* `article.permalink` - use `article.public_url`
* `audio.content_obj_key` - use `audio.article_obj_key`
* `category.url` - use `category.public_url`

