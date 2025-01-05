---
layout: full-width
header-title: Blog
nav_exclude: true
# Note that this index page uses a full-width layout!
---

  <!-- <h1 class="content-listing-header sans"></h1> -->
  <ul class="content-listing ">
    {% assign main_posts = site.posts | where: "main", true %}
    {% assign archive_posts = site.posts | where: "main", false %}
    <h1 class="content-listing-header sans">Context Window</h1>
    {% for post in main_posts %}
        <li class="listing">
          <hr class="slender">
          <div class="post-container">
            <a href="{{ post.url | prepend: site.baseurl }}"><h3 class="post-title contrast">{{ post.title }}</h3></a>
            <span class="smaller">{{ post.date | date: "%B %-d, %Y" }}</span>
          </div>
          <!-- <div>{{ post.excerpt }}</div> -->
        </li>
    {% endfor %}
    <h1 class="content-listing-header sans"></h1>
    <h1 class="content-listing-header sans">Archive</h1>
    {% for post in archive_posts %}
        <li class="listing">
          <hr class="slender">
          <div class="post-container">
            <a href="{{ post.url | prepend: site.baseurl }}"><h3 class="post-title contrast">{{ post.title }}</h3></a>
            <span class="smaller">{{ post.date | date: "%B %-d, %Y" }}</span>
          </div>
          <!-- <div>{{ post.excerpt }}</div> -->
        </li>
    {% endfor %}
  </ul>
