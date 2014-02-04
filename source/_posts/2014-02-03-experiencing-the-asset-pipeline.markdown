---
layout: post
title: "Experiencing the Asset Pipeline"
date: 2014-02-03 22:00:07 -0600
comments: true
categories:
---

Here at PeopleAdmin, we are constantly trying to improve our app. Well, isn't everybody. There are many things that we have done to improve our workflow, our speed, our developer happiness, etc. There is one thing that was still biting us from the old days of Rails, though.

> We hadn't upgraded to the Asset Pipeline.

There were several reasons for this, but the simplest one is that we didn't do a lot of front-end development. We concentrated most of our efforts on the bad-end side of Rails and left the front alone. Sure, we touched it every so often to fix a bug or add a new feature that required fiddling with javascript and stylesheets, but in general we let it alone.

Those days are now gone. More and more our app needed front-end care and organizing. As part of a new project we are working on, a lot of new javascript and styling was introduced and reminded us how languid our scripts and styles were. We needed to change. So now was the perfect time to revisit the dreaded Pipeline.

There were several concerns about switching to the asset pipeline:

1. How can we ensure we are not breaking things?
2. How much manual work is necessary?

Our app is, well, enourmous. We have a lot of javascript and CSS strewn throughout the app from the views to the assets to the layouts. If you know anything about Rails asset organization, you know this is a big no-no. "You put page-specific styles in your view? Are you asking for a migrane in six months?".

Here is how I approached the problem of reducing the number of breaks

## Automate all the things

{% img right /images/automate-all-the-things.jpg 350 %}

Automation was key to our migration to the asset pipeline. Just constructing the script was enough work, but I couldn't have imagined doing it by hand and being confident that I caught all of the exceptions and oddities.
