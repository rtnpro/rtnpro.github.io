+++
author = "Ratnadeep Debnath"
categories = ["endor", "javascript", "kettu", "ratnadeep debnath", "transmission", "patch", "rtnpro"]
date = 2010-04-05T15:56:12Z
description = ""
draft = false
slug = "wrote-a-patch-for-kettu-web-interface-for-transmssion"
tags = ["endor", "javascript", "kettu", "ratnadeep debnath", "transmission", "patch", "rtnpro"]
title = "Wrote a patch for kettu, web interface for transmssion"

+++


Kettu is the new web interface for transmission bit torrent client. It is written using javascript and jquery. I wrote a small patch for kettu to filter the torrents on the basis of activity. Here activity means if the torrent is either uploading or downloading, i.e, upload_speed + download_speed > 0 KBps.

Here’ s the patch I wrote :

> diff –git a/index.html b/index.html  
>  index 2a8ff97..b54b6b4 100644  
>  — a/index.html  
>  +++ b/index.html  
>  @@ -58,6 +58,7 @@  
>  <a href=”#” id=”activate_sorts”>Sort</a> -   
>  <span id=”filters”>  
>  <a href=”#/torrents?filter=all”>All</a>  
>  +            <a href=”#/torrents?filter=active”>Activity</a>  
>  <a href=”#/torrents?filter=downloading”>Downloading</a>  
>  <a href=”#/torrents?filter=seeding”>Seeding</a>  
>  <a href=”#/torrents?filter=paused”>Paused</a>  
>  @@ -94,4 +95,4 @@  
>  </nav>  
>  </footer>  
>    
>  -  
>  No newline at end of file  
>  +  
>  diff –git a/js/helpers/filter_torrents_helpers.js b/js/helpers/filter_torrents_helpers.js  
>  index 95f841e..3b54b2c 100644  
>  — a/js/helpers/filter_torrents_helpers.js  
>  +++ b/js/helpers/filter_torrents_helpers.js  
>  @@ -5,6 +5,12 @@ var FilterTorrentsHelpers = {
> 
> if(filter_mode == ‘all’) {  
>  filtered_torrents = torrents;  
>  +    } else if(filter_mode == ‘active’) {  
>  +      $.each(torrents, function() {  
>  +        if(this.activity()) {  
>  +          filtered_torrents.push(this)  
>  +        }  
>  +      })  
>  } else {  
>  $.each(torrents, function() {  
>  if(this.status == stati[filter_mode]) {  
>  @@ -15,4 +21,4 @@ var FilterTorrentsHelpers = {
> 
> return filtered_torrents;  
>  }  
>  -}  
>  No newline at end of file  
>  +}  
>  diff –git a/js/models/torrent.js b/js/models/torrent.js  
>  index 8e39f1d..1bf2a2a 100644  
>  — a/js/models/torrent.js  
>  +++ b/js/models/torrent.js  
>  @@ -106,6 +106,7 @@ Torrent = function(attributes) {  
>  localized_stati[torrent.stati[‘downloading’]] = ‘Downloading';  
>  localized_stati[torrent.stati[‘seeding’]] = ‘Seeding';  
>  localized_stati[torrent.stati[‘paused’]] = ‘Paused';  
>  +    localized_stati[torrent.stati[‘active’]] = ‘Activity';
> 
> return localized_stati[this[‘status’]] ? localized_stati[this[‘status’]] : ‘error';  
>  };  
>  @@ -138,4 +139,4 @@ Torrent = function(attributes) {  
>  };
> 
> return torrent;  
>  -};  
>  No newline at end of file  
>  +};

The patch got accepted by the kettu upstream **endor** on  2010-03-29 and following is the patch commit link:

[http://github.com/endor/kettu/commit/5d3a64c4807eee6bbfbb2d3013e384971930bca8](http://github.com/endor/kettu/commit/5d3a64c4807eee6bbfbb2d3013e384971930bca8)

Feels nice to have written my first patch ![:)](http://127.0.0.1:8080/wordpress/wp-includes/images/smilies/icon_smile.gif)

