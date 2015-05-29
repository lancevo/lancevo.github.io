---
layout: post
title:  "Scaling background image with CSS"
date:   2015-05-29 17:06:00
categories: css
tags: responsive
image: 
published: true
---

A quick snippet of CSS to scale background image, and seems to work pretty well. The trick is using `padding-top:<ratio of height/width>%`.
Since my images are square so its ratio is 100%;

```css
.scale-img {
   background-repeat: no-repeat;
   background-size: 100%;
   padding-top: 100% 
}

#img-1 {
   background-image:url('img1.png');
}
#img-2 {
   background-image:url('img2.png');
   }
```
