---
layout: page
title:  "PODCASTING PEARLS"
date:   2019-08-13 21:03:36 +0530
categories: misc
lang: en
ref: podcasting-pearls
---
## Podcasting pearls ✨

Podcasts are going through a boom. 
Independent, professionals, tech, marketing, sales, radios, 
everybody's rushing on this format and it's getting hard to get signal through the noise.   

At some point I realized I wanted to have my podcasts notes close to the content.    
It did not find this in any app, so I started with my personal site.   

I wish one day we'll have "underline" and "surround" features in podcast apps.  
In  the meantime, I'll stick to this website.   
You can find podcast articles titled "Podcasting Pearls #...", it's a reference to [Programming Pearls](https://www.amazon.com/Programming-Pearls-2nd-Jon-Bentley/dp/0201657880).  
There is a mix of engineering, economics, psychology, but you should mostly expect data engineering.     
Good listening ! 


**List of podcasting pearls:**
<ul>
  {% for post in site.posts %}
    {% if post.title contains 'Podcasting Pearl' %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
    {% endif %}
  {% endfor %}
</ul>

