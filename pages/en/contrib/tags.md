---
title: Tags
audience: writer, designer
tags: [navigation]
last_updated: July 3, 2016
keywords: tags, navigation, buttons, links, association
summary: "Tags provide another means of navigation for your content. Unlike the table of contents, tags can show the content in a variety of arrangements and groupings. Implementing tags in this Jekyll theme is somewhat of a manual process."
sidebar: contrib_sidebar
permalink: /doc/en/contrib/tags.html
folder: contrib
---

## Add a tag to a page
You can add tags to pages by adding `tags` in the frontmatter with values inside brackets, like this:

```
---
title: 5.0 Release Notes
permalink: /doc/en/contrib/release_notes_5_0.html
tags: [formatting, single_sourcing]
---
```

## Tags overview

To prevent tags from getting out of control and inconsistent, first make sure the tag appears in the `\_data/tags.yml` file. If it's not there, the tag you add to a page won't be read. This helps to ensure that you
use tags consistently and don't add new tags without the corresponding tag archive pages.

Additionally, you must create a tag archive page similar to the other pages named `tag_{tagname}.html` in the `tags` folder. This theme doesn't auto-create tag archive pages.

For simplicity, make all your tags single words (connect them with hyphens if necessary).

{% include note.html content= "
We've modified the original theme's tag setup as follows:

- Tag archive pages must have a `lang` property that specifies the language (e.g. `en`).  The page will list only pages that match that language, since you probably only want to view pages of your own language.  
- Tag archive pages will list only pages that also match the `sidebar` property.  This ensures that the tag archive displays only pages relevant to the same product, for example LoopBack 2.x.
- Above means that each lanuage + product must have a `tags` folder with tag archive files, e.g. `en/lb2` and `en/lb3` each have a `tags` folder with the requisite tag archive files.
- Tag archive lists only pages, not posts (since we're not using posts on this site), and that's removed from the table generated by `taglogic.html`.
" %}

## Setting up tags

Tags have a few components.

1. In the `\_data/tags.yml` file, add the tag names you want to allow. For example:

   ```json
   allowed-tags:
     - getting_started
     - overview
     - formatting
     - publishing
     - single_sourcing
     - special_layouts
     - content types
   ```

3. Create a tag archive file in the `tags` folder for each tag in your `tags_doc.yml` list. Name the file following this pattern: `tag_collaboration.html`.

   Each tag archive file needs only this:

   {% raw %}
   ```liquid
---
title: "Collaboration pages"
tagName: collaboration
search: exclude
permalink: /doc/en/contrib/tag_collaboration.html
sidebar: contrib_sidebar
---
{% include taglogic.html %}
   ```
    {% endraw %}

   {% include note.html content="In the \_includes/mydoc folder, there's a taglogic.html file. This file (included in each tag archive file) has common logic for getting the tags and listing out the pages containing the tag in a table with summaries or truncated excerpts. You don't have to do anything with the file &mdash; just leave it there because the tag archive pages reference it.
   " %}

4. Change the title, tagName, and permalink values to be specific to the tag name you just created.

   By default, the \_layouts/page.html file will look for any tags on a page and insert them at the bottom of the page using this code:


```liquid
{% raw %}<div class="tags">
{% if page.tags != null %}
<b>Tags: </b>
{% assign projectTags = site.data.tags.allowed-tags %}
{% for tag in page.tags %}
{% if projectTags contains tag %}
<a href="{{ "tag_" | append: tag | append: ".html" }}" class="btn btn-default navbar-btn cursorNorm" role="button">{{page.tagName}}{{tag}}</a>
{% endif %}
{% endfor %}
{% endif %}
</div>{% endraw %}
```


Because this code appears on the \_layouts/page.html file by default, you don't need to do anything in your page to get the tags to appear. However, if you want to alter the placement or change the button color, you can do so within the \_includes/taglogic.html file.

You can change the button color by changing the class on the button from `btn-info` to one of the other button classes bootstrap provides. See [Labels][mydoc_labels] for more options on button class names.

## Retrieving pages for a specific tag

If you want to retrieve pages outside of a particular tag_archive page, you could use this code:

{% raw %}
```liquid
navigation pages:
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "navigation" %}
<li><a href="{{page.permalink}}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
```
{% endraw %}

Here's how that code renders:

navigation pages:
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "navigation" %}
<li><a href="{{page.permalink}}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>

If you want to sort the pages alphabetically, you have to apply a `sort` filter:

```liquid
{% raw %}
navigation pages:
<ul>
{% assign sorted_pages = (site.pages | sort: 'title') %}
{% for page in sorted_pages %}
{% for tag in page.tags %}
{% if tag == "navigation" %}
<li><a href="{{page.permalink}}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
{% endraw %}
```

Here's how that code renders:

navigation pages:
<ul>
{% assign sorted_pages = (site.pages | sort: 'title') %}
{% for page in sorted_pages %}
{% for tag in page.tags %}
{% if tag == "navigation" %}
<li><a href="{{page.permalink}}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>

## Efficiency
Although the tag approach here uses `for` loops, these are somewhat inefficient on a large site. Most of my tech doc projects don't have hundreds of pages (like my blog does). If your project does have hundreds of pages, this `for` loop approach with tags is going to slow down your build times.

Without the ability to access pages inside a universal namespace with the page type, there aren't many workarounds here for faster looping.

With posts (instead of pages), since you can access just the posts inside `posts.tag.tagname`, you can be a lot more efficient with the looping.

Still, if the build times are getting long (e.g., 1 or 2 minutes per build), look into reducing the number of `for` loops on your site.

## Empty tags?

If your page shows "tags:" at the bottom without any value, it could mean a couple of things:

* You're using a tag that isn't specified in your allowed tags list in your tags.yml file.
* You have an empty `tags: []` property in your frontmatter.

If you don't want tags to appear at all on your page, remove the tags property from your frontmatter.

## Remembering the right tags

Since you may have many tags and find it difficult to remember what tags are allowed, I recommend creating a template that prepopulates all your frontmatter with all possible tags. Then just remove the tags that don't apply.

{% include links.html %}