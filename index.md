---
title: "Quentin Fortier"
layout: splash
sticky: false
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/woods.png
excerpt: "PhD and teacher in Computer Science"  
---

<link rel="stylesheet" href="https://unpkg.com/octicons@4.4.0/build/font/octicons.css">
<link rel="stylesheet" href="https://unpkg.com/github-activity-feed@latest/dist/github-activity.min.css">

<script type="text/javascript" src="https://unpkg.com/mustache@4.2.0/mustache.min.js"></script>
<script type="text/javascript" src="https://unpkg.com/github-activity-feed@latest/dist/github-activity.min.js"></script>

<!-- <center>
  <img src="/assets/images/qf.png" style="max-width: 180px; border-radius: 50%;" alt="Quentin Fortier"> 
  <div class="half-line"><br></div>
</center> -->

<h3 class="archive__subtitle">Last posts</h3>
{% if paginator %}
  {% assign posts = paginator.posts %}
{% else %}
  {% assign posts = site.posts %}
{% endif %}

{% assign entries_layout = page.entries_layout | default: 'list' %}
<div class="entries-{{ entries_layout }}">
  {% for i in (0..4) %}
  {% assign post = posts[i] %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>

<!-- <div id="feed"></div>

<script type="text/javascript">
GitHubActivity.feed({
  username: "fortierq",
  //repository: "your-repo", // optional
  selector: "#feed",
  limit: 5,
});
</script> -->
