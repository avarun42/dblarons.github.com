---
layout: page
title: Start Learning to Code
tagline: Supporting tagline
---
{% include JB/setup %}

Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

## My Goal

Code is changing the world. With it, you can create websites, crunch numbers, and automate monotonous tasks. But there is a stigma that programming is a difficult undertaking only to be tackled by the biggest nerds. That stigma is wrong, and to prove that it's wrong, I'm going to teach you how to code.

The reality is, programming can be exciting, thought provoking, and downright fun. And I bet you already know more than you think.

    if my_goal == your_goal:
        self.teach_you_to_program(you)
        you.profit

Read the above. If that makes sense to you, then you can be a programmer! Pretty soon, you'll be able to write programs ten times that length. Let's get started.
    
## Recent Posts

If you can't find what you're looking for here, check out the archive.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


