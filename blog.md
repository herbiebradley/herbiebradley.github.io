---
layout: full-width
header-title: Blog
# Note that this index page uses a full-width layout!
---

  <h1 class="content-listing-header sans"></h1>
  <ul class="content-listing">
    {% assign left_posts = site.posts | where: "main", true %}
    {% assign right_posts = site.posts | where: "main", false %}
    <li class="left-column">Main Sequence
      <hr class="slender">
        {% for post in left_posts %}
            <div class="post-container">
                <a href="{{ post.url | prepend: site.baseurl }}"><h3 class="post-title contrast">{{ post.title }}</h3></a>
                <span class="smaller">{{ post.date | date: "%B %-d, %Y" }}</span>
            </div>
        {% endfor %}
    </li>
    <li class="right-column">Archive
      <hr class="slender">
        {% for post in right_posts %}
            <div class="post-container">
                <a href="{{ post.url | prepend: site.baseurl }}"><h3 class="post-title contrast">{{ post.title }}</h3></a>
                <span class="smaller">{{ post.date | date: "%B %-d, %Y" }}</span>
            </div>
        {% endfor %}
    </li>
</ul>
