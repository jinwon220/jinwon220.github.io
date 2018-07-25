---
title: Javascript(12)_JSON_API
author: JinWon
layout: post
category: JavaScript
---

# JavaScript_JSON_API

~~~
아래 JSON 객체에서 m이라는 속성 image 파일을 출력하세요
https://www.flickr.com/services/api/
http://api.flickr.com/services/feeds/photos_public.gne?tags=raccoon&tagmode=any&format=json&jsoncallback=?
~~~

~~~javascript
var data = {
				"title": "Recent Uploads tagged raccoon",
				"link": "https:\/\/www.flickr.com\/photos\/tags\/raccoon\/",
				"description": "",
				"modified": "2018-03-14T01:00:45Z",
				"generator": "https:\/\/www.flickr.com",
				"items": [
						   {
								"title": "Raccoon June 2017",
								"link": "https:\/\/www.flickr.com\/photos\/11799527@N08\/40797274301\/",
								"media": {"m":"https:\/\/farm5.staticflickr.com\/4782\/40797274301_b4fe4a9e46_m.jpg"},
								"date_taken": "2017-06-04T12:05:13-08:00",
								"description": " <p><a href=\"https:\/\/www.flickr.com\/people\/11799527@N08\/\">coomare<\/a> posted a photo:<\/p> <p><a href=\"https:\/\/www.flickr.com\/photos\/11799527@N08\/40797274301\/\" title=\"Raccoon June 2017\"><img src=\"https:\/\/farm5.staticflickr.com\/4782\/40797274301_b4fe4a9e46_m.jpg\" width=\"240\" height=\"232\" alt=\"Raccoon June 2017\" \/><\/a><\/p> <p>Procyon lotor<br \/> Memorial Lake State Park <br \/> Lebanon Co PA <br \/> 20170604-IMG_8142-2.jpg<\/p>",
								"published": "2018-03-14T01:00:45Z",
								"author": "nobody@flickr.com (\"coomare\")",
								"author_id": "11799527@N08",
								"tags": "lebanoncounty memoriallakestatepark pastatepark pennsylvania animal raccoon"
						   },
                           .
                           .
                           .
~~~
~~~javascript
window.onload = function(){
    var img = document.getElementById("image");
    /* 
    for(var index in data.items){
        img.src = data.items[index].media.m;
    }
        */
    var index = 0;
    img.src = data.items[index].media.m;
    //console.log(data.items.length);
    
    document.getElementById("next").onclick = function(){
        if(index < data.items.length-1){
            ++index;
        }else{index = 0;}
        img.src = data.items[index].media.m;
    }
    
    document.getElementById("prv").onclick = function(){
        if(index > 0){
            --index;
        }else{index = data.items.length-1;}
        img.src = data.items[index].media.m;
    } 
}
~~~