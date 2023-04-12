# jekyll-minimag: A skeleton Jekyll-powered static site for a blog

## Overview

A work in progress…

A Jekyll site based on the minima theme (version 2.5.1). (The dependence on the gem theme has been overridden, so updates to minima will have no effect on Minimag.)

This repo is designed for development within a Docker container.

The initial construction of this site follows Bill Raymond’s YouTube video “[Draft training - Run GitHub Pages in a Docker container](https://www.youtube.com/watch?v=4zCZhPjzlc0&lc=Ugw9B54_UzDEPIQFP_N4AaABAg),” which was a draft and unlisted as of April 2, 2023.

## Features (that differentiate Minimag from minima)
### Each blog post is represented on listings pages by a “Card”
One departure of Minimag from minima is the implementation of a “card” presentation, including an illustration, for each post’s entry on posts-listing pages. A card has the following elements:
* An image component
* A text component, made up of subcomponents
  * Post title
  * Post excerpt
  * Post date (The last-updated date)
  * Post author

* A “featured post” will have a horizontal card format such that the image will occupy the top of the card, and the text elements will appear below the image
  * All featured-post cards will be the same width, and narrow enough so that on a desktop display, multiple cards will typically span a row of cards for featured posts.
* The posts in a list of not necessarily featured posts will have a vertical card format such that the image will occupy the left part of the card, and the text elements will appear in the right side of the card.
  * Each vertical-format card will span the full width of the display.

### The URLs of posts do *not* encode the post’s date or the post’s category
In its out-of-the-box configuration, the URL for a post `2023-04-02-welcome-to-jekyll.markdown` is simply `/welcome-to-jekyll`.

This is in contrast to the default behavior of, the built-in theme minima, where the URL for a post encodes the post’s date and categories. E.g., when the categories are “jekyll” and “update”: `/jekyll/update/2023/04/02/welcome-to-jekyll.html`.

This decision for Minimag was made primarily to allow the date of a post to be updated without changing its URL.

Note: This decision implies that posts now share the same namespace as pages, so there is the possibility of collisions. Names of posts and pages must be chosen to avoid such a collision.

Related to this: The `_posts` directory is now flat: there are not spearate folders by date and/or category.

### More flexibility in defining  header structures and allowing for different header structures for different pages/posts
The original default layout assumed:
* that all posts and pages would have the same header/navbar structure
* that the navbar element was always the topmost element of the page, preventing a banner graphic appearing above the navbar.

In Minimag, the default layout is less opininated about the header. The structure of the header is now not defined by the default layout but rather by each other layout that inherit from default. This gives greater flexibility, for example,  in placing the navbar either above or below a header banner and/or having a different header structure for, for example, the home page than a post page.


## Selective file tree
```
├── _includes
├── _layouts
├── _pages
├── _posts
│   └── 2023-04-02-welcome-to-jekyll.markdown
├── _sass
├── about.md
├── assets
│   ├── images
│   │   └── posts
│   │       ├── 2023-04-02-Oakland-Bay-Bridge-east-span-photo-1575649212494-720dd18f9c35.avif
│   ├── main.scss
│   └── minima-social-icons.svg
└── index.md
```

## Usage notes
### Posts
#### By default, posts use layout `post.html`
By default (due to a setting in `_config.yml`), every post will inherit the layout `post.html`.

Thus it is not necessary to include `layout: post` in the front matter of a post.

#### Images for posts
There are three types of images for posts: (a) `card-image`, (b) `top-image`, and (c) interior images. Types (a) and (b) require special attention in the post’s front matter; type (c) does not.
* `card-image`
  * An image associated with a particular post to be displayed in the *card* for that post on a posts-listing page, such as the home page, a category listing page, or an archive page.
  * To assure uniformity across posts’ cards, there will be a fixed aspect ratio that applies to all posts’ cards and their images.
    * The displayed image will be displayed with `object-fit: cover`, because this (a) does not distort/skew the image’s native aspect ratio and (b) completely fills the alloted space without letterboxing. (See Jim Ratliff, [“How the object-fit CSS property works: Comparing the various values](https://codepen.io/jimratliff/pen/wVYrKb),” CodePen.)
  * To assure uniformity across posts’ cards, an image *will* be displayed in each post’s card, even if the post does not specify a `card-image`.
  * The post-card image displayed depends at least on the value of `card-image`. If `card-image` is
    * specified, the specified image will be displayed
    * `default`, the default post-card image will be displayed
      * Note: Although this is the default behavior even when `card-image` is unspecified, specifying `default` can have an effect when the post *does* have a specification for `top-image`. In this case, specifying `card-image` as `default` prevents the specified `top-image` from being displayed in the post’s card.
    * unspecified:
      * If a `top-image` for the post is specified,
        * that image is used in the post’s card
      * Otherwise:
        * the default post-card image will be displayed
* `top-image`
  * An image displayed at the top of a post.
    * An image in this location requires special attention in the front matter of the post because this image will appear *prior* to the content in the post file. This requires the file name of the image to be passed to `_layouts/post.html` so that the image can be rendered in the header portion of that layout.
  * There is no aspect-ratio constraint for a `top-image`. Its full height will be displayed at the top of a post, and the remaining content of the post will begin immediately below the bottom of the `top-image`.
  * A `top-image` is not mandatory. Thus,
    * If a `top-image` is specified, it is used at the top of the post.
    * If a `top-image` is not specified, an image is not displayed at the top of the post.
  * The top image also has an associated, optional, `top-image-caption`.
* Interior images
  * These are images displayed in the body of a post.
  * These don’t require special attention in the front matter, because the image is rendered within the content of the post’s file.

Usage notes:
* Ideally, whatever image plays the role of `card-image` should have a native aspect ratio close to that of the frame in the card into which it must fit.
* If the same image should be used for both `card-image` and `top-image`, it is sufficient to specify that image for `top-image`, and leave `card-image` unspecified. In that case the image for `top-image` will be chosen for `card-image`.

#### Location for post-specific images
It is intended that images used only in particular posts should be located here:
`assets/images/posts`

`config.yml` now defines two related variables:
```yaml
url_images: "/assets/images"
url_post_images: "/assets/images/posts"
```

#### Interior images
See the post 2023-04-06-displaying-interior-images-with-markdown.markdown for ways that an image can be included in the interior of a post using Markdown syntax:

### License

This site structure is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

## Release history
* v0.0.1, April 2, 2023
  * Essentially identical to the default, built-in minima theme, except that theme is frozen at version 2.5.1.
  * Configured for development within a Docker container.
* v.0.0.2, April 4, 2023
  * Sets the locale (and UTF-8) for Docker container
* v.0.0.3, April 4, 2023
  * `post.html` is now the default layout for posts. (No need to specify in front matter of a post.)
  * URL of a post is now simply the sluggified title; URL doesn’t encode the data or any categories. Ditto for the `_posts` directory: it’s flat.
* v.0.0.4, April 6, 2023
  * Refactored default layout to be less opinionated. Original default layout assumed that all posts and pages would have the same header/navbar structure. This decision has been moved out of the default layout, into the hands of the other layouts that inherit from default. This gives greater flexibility (a) in placing the navbar either above or below a header banner and (b) to having a more-expansive branding banner image on a home and/or listing page but a smaller branding image on a post page (so as not to distrct from the post-specific featured image).
  * Created `site.description_for_display` in `config.yml` to leave `site.description` exclusively for the jekyll-seo-tag plugin.
  * Created assets/images/posts directory to be used for storing all images that are specific to particular posts.
  * Added an `updated` datetime for posts (though it still needs to be implemented on each post) and it not yet connected to any sorting functionality.
  * Added an example post that shows ways to display an interior image (i.e., within a post’s content) using Markdown syntax.
* v.0.0.5, April 9, 2023
  * Moved the `<nav>` landmark role, which had been nested within the banner landmark role, `<header>`, to an independent top-level role.
    * There’s an accessibility argument for this.
  * The navbar has been removed from `_includes/header.html` and into its own `_includes/nav-role.html`.
    * This gives greater flexibility is placing the navbar either above or below a header banner.
    * The navbar no longer automatically populates with all pages.
    * Instead, you manually populate, in `_includes/nav-role.html`, the list items you want to be represented on the navbar.
  * There is no longer a logo or site title reference in the navbar.
    * See `_includes/header_A.html` for how (a) a standalone strip for the site title and/or a logo and (b) the navbar can be separately included in either order.
* v.0.0.6, April 10, 2023
  * Add ability to specify an image in the front matter (`top-image`) to be displayed at the top of each post (above the post’s title). You can specify alt text with `top-image-alt`.
    * Optionally, specify text (`top-image-caption`), which can be multi-line, to appear directly below the top-image as a caption.
* v.0.0.7, April 10, 2023
  * Add CSS custom property (--nav-toggle-label-vertical-offset-fudge-term) to adjust the vertical position of the “hamburger” navicon.
* v.0.0.8, April 11, 2023
  * Style .table-of-contents
* Pending


## To Dos and outstanding issues
### Add a site search function
I should probably use either Google or DuckDuckGo. See “[The easiest search option for Jekyll](https://blog.webjeda.com/jekyll-search/),” WebJeda, July 18, 2016.
### jekyll-seo-tag
Minimag inherits from minima the jekyll-seo-tag plugin. From README_minima.md:

> Minima comes with [`jekyll-seo-tag`](https://github.com/jekyll/jekyll-seo-tag) plugin preinstalled to make sure your website gets the most useful meta tags. See [usage](https://github.com/jekyll/jekyll-seo-tag#usage) to know how to set it up.

The [usage docs](https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md) for jekyll-seo-tag identify a large number of site-wide variables that this plugin will exploit when they’re present, including `title`, `tagline`, `description`, `url`, `author`, `twitter` (including `twitter:card` and `twitter:username`), `facebook` (and associated properties), `logo`, `social` (and associated properties), `google_site_verification`, and `locale`.

At the page/post level, tThe SEO tag will respect the following YAML front matter if included in a post, page, or document:

* `title` - The title of the post, page, or document
* `description` - A short description of the page's content
* `image` - URL to an image associated with the post, page, or document (e.g., `/assets/page-pic.jpg`)
* `author` - Page-, post-, or document-specific author information (see [Advanced usage](advanced-usage.md#author-information))
* `locale` - Page-, post-, or document-specific locale information. Takes priority over existing front matter attribute `lang`.

*Note:* Front matter defaults can be used for any of the above values as described in advanced usage with an image example.

*Questions:*
* Since one of my personal use cases is to use this blog as a *complement* to a main site (i.e., being accessed by visitors at TLD/blog, via Cloudflare workers after being served directly at blog.TLD subdomain), I have to consider how much of this plugin’s site-wide function conflicts with what I’m already (or should be) doing on the main site.
* Is the post-specific SEO stuff worthwhile? Doesn’t Google do a good-enough job on this?