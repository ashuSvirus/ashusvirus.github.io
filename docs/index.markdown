---
layout: default
title: "Ashutosh Mittal - Backend Engineer"
---

# Welcome to My Technical Blog

I'm Ashutosh Mittal, a Backend Engineer and IIT Roorkee graduate with expertise in infrastructure scaling and security.

## About Me

I have been working in backend development for the last 6 years professionally. I started my development journey with HTML and CSS. After 1 year, I picked up PHP for backend development learning. For 2 years I worked with PHP, used frameworks like Laravel and CakePHP. 

My area of interest is backend engineering, infrastructure scaling and security.

## Recent Posts

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
{% endfor %}

## Connect With Me

- [GitHub](https://github.com/ashuSvirus)
- [Twitter](https://twitter.com/ashuSvirus)  
- [LinkedIn](https://www.linkedin.com/in/ashusvirus/)
