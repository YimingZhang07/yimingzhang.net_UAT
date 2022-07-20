---
title: Website Deployment Notes
subtitle:

# Summary for listings and search engines
summary: Some notes when configuring the website.

# Link this post with a project
projects: []

# Date published
date: '2022-07-01T00:00:00Z'

# Date updated
lastmod: '2020-07-01T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

tags:
  - Website

categories:
  - Tricks
---

## Netlify

Build the website using a specific version of Hugo. Need to edit the `netlify.toml` file inside the root folder.

```toml
[build.environment]
  HUGO_VERSION = "0.96.0"
  HUGO_ENABLEGITINFO = "true"
```

Some of the pages embed the `html` or `pdf` files in a `iFrame` window, it will be blocked by default from the Netlify. To enable this feature, also go to this file, and add:

```toml
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "SAMEORIGIN"
    X-XSS-Protection = "1; mode=block"
```

## Embed PDF

Instead of using the `iFrame` to embed the pdf. Here we adopt the adobe API. To shorten the code, we introduce the `\layouts\shortcodes` that provides a template for a block of codes.

The Adobe API is domain specific, so the `localhost` and `netlify.app` domains need two separate APIs. The short code could have a `if/else` statement to distinguish the domain and choose the right API. In the `adobe_inline.html` file, we provide the following logic to see if the current server is ran by `hugo server`.

```javascript
<!-- adobe developer console: https://developer.adobe.com/console/projects -->
<!-- hugo shortcodes: https://gohugo.io/templates/shortcode-templates/ -->

{{if .Site.IsServer}}
{{else}}
{{end}}
```

In each statement, provide the following API, but leave the filename as a parameter. When calling this short code from the markdown, the file name will be forwarded.

```javascript
<!-- To use in markdown: <adobe_inline FILENAME> -->

<div id="adobe-dc-view" style="height: 600px; width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function() {
        var adobeDCView = new AdobeDC.View({
            clientId: "2f4f441ef6f741d988a5fcf60d418dd1",
            divId: "adobe-dc-view"
        });
        adobeDCView.previewFile({
            content: {
                location: {
                    url: "./{{ index .Params 0 }}"
                }
            },
            metaData: {
                fileName: "{{ index .Params 0 }}"
            }
        }, {
            embedMode: "SIZED_CONTAINER"
        });
    });
</script>
```

