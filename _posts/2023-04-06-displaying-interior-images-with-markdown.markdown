---
# layout: post
title:  "Different ways to display interior images using Markdown"
date:  2023-04-06 14:17:03 -0700
categories: 
---
There are multiple ways to display an interior image (i.e., an image in the body of a post, rather than an image above the title of the post).

<!-- Add a Table of Contents -->
<div style="table-of-contents" markdown="1">
Table of contents
* TOC
{:toc}
</div>

## Constructing the full URL, i.e., without using relative_url
### Using only the built-in site.url and site.baseurl variables
<!-- {% raw %} -->
The following renders the image using Markdown with a full URL constructed by “{{site.url}}/{{site.baseurl}}/assets/images/posts/”:

```
![Bay Bridge]({{site.url}}/{{site.baseurl}}/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif)
```
<!-- {% endraw %} -->

![Bay Bridge]({{site.url}}/{{site.baseurl}}/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif)

### Adding the custom site.url_post_images variable
It’s no fun (not to mention error prone) to have type `/assets/images/posts/` as part of the path every time we want to display an image.

So I defined a new, site-wide variable `url_post_images` in `_config.yml`:
```
url_post_images: "/assets/images/posts"
```
So we can now build the full URL by incorporating a reference to this new site-wide variable (instead of explicitly typing the string for which it stands: `/assets/images/posts/`).

Now, if ever the file/folder structure of the site changed, and the images for posts were moved, a single change to `_config.yml` would have back in business in no time.

<!-- {% raw %} -->
The following renders the image using Markdown with a full URL constructed by “{{site.url}}/{{site.baseurl}}{{site.url_post_images}}/”:

```
![Bay Bridge]({{site.url}}/{{site.baseurl}}{{site.url_post_images}}/2023-04-02-Oakland-Bay-Bridge-east-span.avif)
```
<!-- {% endraw %} -->

![Bay Bridge]({{site.url}}/{{site.baseurl}}{{site.url_post_images}}/2023-04-02-Oakland-Bay-Bridge-east-span.avif)

## Constructing a relative URL for the image

The following renders the image using Markdown with a relative URL constructed by
<!-- {% raw %} -->
“ { {"/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif"| relative_url }}”
<!-- {% endraw %} -->
using `relative_url`:

![Bay Bridge]({{"/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif"| relative_url }})

However, I haven’t yet found a syntax by which I can both (a) use `relative_url` and (b) use `site.url_post_images` to avoid typing `/assets/images/posts/`.
