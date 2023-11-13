+++
author = "Ratnadeep Debnath"
categories = ["django", "django-tagging", "indifex", "ratnadeep debnath", "related tag cloud", "related tags", "tag cloud", "tag graph", "transifex", "rtnpro"]
date = 2011-10-10T17:13:22Z
description = ""
draft = false
slug = "transifex-implements-related-tag-cloud"
tags = ["django", "django-tagging", "indifex", "ratnadeep debnath", "related tag cloud", "related tags", "tag cloud", "tag graph", "transifex", "rtnpro"]
title = "Transifex implements related tag cloud"

+++


Lately, I have been working on a bunch of exciting new stuffs for Transifex. I have worked on a tag-cloud implementation which not only shows the popular tags, but also shows tags related to a tag selected by the user. It is pretty useful. It directs the user to select more relevant tags. The tag cloud is refreshed each time the user makes a selection to show the related tags.

I built this on top of the django-tagging module. I wrote a model to represent a tag as a node in a graph. The model includes all the tags related to it (that is tags which appear with the tag in concern) as adjacent nodes along with the weight (that is number of times the two tags appear together) of each edge between two related tag nodes. This data is updated and synced as necessary, e.g, after a project is added or updated. Now, whenever a tag is selected, the tag-cloud is refreshed to show the related tags. The font-size of a related tag is decided by taking into consideration both the weight of an edge it shares with the selected tag and its count. Below is a sample use case for related tag-cloud in Transifex.

Let’s say there are two projects, p1 with tag ‘foo1′ and p2 with tags ‘gui’, ‘graphics’, ‘imaging’ and ‘photography’. For sake of simplicity, I am showing only 3 most popular tags: ‘foo1′, ‘gui’, ‘graphics’. So, now when the maintainer for prohect p1 goes to edit the project, he sees the following tag-cloud:

<figure class="wp-caption aligncenter" id="attachment_304" style="width: 300px;">[![](http://127.0.0.1:8080/wordpress/wp-content/uploads/2011/10/tag_cloud1.png?w=300 "Initial tagcloud")](http://127.0.0.1:8080/wordpress/wp-content/uploads/2011/10/tag_cloud1.png)<figcaption class="wp-caption-text">Initial tagcloud</figcaption></figure>Now, he selects a new tag ‘graphics’ and the tagcloud is refreshed to show the tags related to ‘graphics’.

<figure class="wp-caption aligncenter" id="attachment_305" style="width: 300px;">[![](http://127.0.0.1:8080/wordpress/wp-content/uploads/2011/10/tag_cloud2.png?w=300 "Tagcloud with related tags")](http://127.0.0.1:8080/wordpress/wp-content/uploads/2011/10/tag_cloud2.png)<figcaption class="wp-caption-text">Tagcloud with related tags.</figcaption></figure>Such small things together can really take the user experience to a new level. By implementing related tag-clouds, we enable the user to choose relevant tags in a better way. At Transifex, we innovate to help people localize in a better way :).

