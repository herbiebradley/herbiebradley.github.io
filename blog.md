---
layout: full-width
header-title: Blog
# Note that this index page uses a full-width layout!
---

  <!-- <h1 class="content-listing-header sans"></h1> -->
  <ul class="content-listing ">
    {% for post in site.posts %}
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
