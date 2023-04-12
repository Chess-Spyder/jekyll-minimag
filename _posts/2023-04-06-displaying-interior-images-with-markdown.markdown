---
# layout: post
title:  "Different ways to display interior images using Markdown"
date:  2023-04-06 14:17:03 -0700
top-image: 2023-04-06-raptor_for_top_of_post_16x9.jpg
top-image-alt: Raptor flying in a blue sky.
top-image-caption: >-
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
  eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
  veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
  consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum
  dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum

categories: 
---
There are multiple ways to display an interior image (i.e., an image in the body of a post, rather than an image above the title of the post).

<!-- Add a Table of Contents -->
Table of contents
<div class="table-of-contents" markdown="1">
1. TOC
{:toc}
</div>

## Constructing the full URL, i.e., without using relative_url
### Using only the built-in site.url and site.baseurl variables
<!-- {% raw %} -->
The following renders the image using Markdown with a full URL constructed by “{{site.url}}/{{site.baseurl}}/assets/images/posts/”:

```
![Bay Bridge]({{site.url}}{{site.baseurl}}/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif)
```
<!-- {% endraw %} -->

![Bay Bridge]({{site.url}}{{site.baseurl}}/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif)

### Adding the custom site.url_post_images variable
It’s no fun (not to mention that it’s error prone) to have to type `/assets/images/posts/` as part of the path every time we want to display an image.

So I defined a new, site-wide variable `url_post_images` in `_config.yml`:
```
url_post_images: "/assets/images/posts"
```
So we can now build the full URL by incorporating a reference to this new site-wide variable (instead of explicitly typing the string for which it stands: `/assets/images/posts/`).

Now, if ever the file/folder structure of the site changed, and the images for posts were moved, a single change to `_config.yml` would have back in business in no time.

<!-- {% raw %} -->
The following renders the image using Markdown with a full URL constructed by “{{site.url}}/{{site.baseurl}}{{site.url_post_images}}/”:

```
![Bay Bridge]({{site.url}}{{site.baseurl}}{{site.url_post_images}}/2023-04-02-Oakland-Bay-Bridge-east-span.avif)
```
<!-- {% endraw %} -->

![Bay Bridge]({{site.url}}{{site.baseurl}}{{site.url_post_images}}/2023-04-02-Oakland-Bay-Bridge-east-span.avif)

## Constructing a relative URL for the image

The following renders the image using Markdown with a relative URL constructed by
<!-- {% raw %} -->
“ { {"/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif"| relative_url }}”
<!-- {% endraw %} -->
using `relative_url`:

![Bay Bridge]({{"/assets/images/posts/2023-04-02-Oakland-Bay-Bridge-east-span.avif"| relative_url }})

However, I haven’t yet found a syntax by which I can both (a) use `relative_url` and (b) use `site.url_post_images` to avoid typing `/assets/images/posts/`.
