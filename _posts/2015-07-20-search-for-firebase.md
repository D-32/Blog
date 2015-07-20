---
layout: post
title:  "Search for Firebase"
date:   2015-07-20 13:00:00
categories: web
---
I wanted a simple way to do searches on my firebase content. Nodejs seemed the way to go, as setting up a server is easy and it works great with Firebase too. As a search engine I decided to use lunr.js, it's fast and easy to setup.

### Installation
	npm install firebase
	npm install lunr
	
Next we need some imports:

	var Firebase = require("firebase");
	var lunr = require("lunr");
	var http = require('http');
	var url = require('url');
	
lunr.js requires to define the structure of the index:

	var index = lunr(function () {
    	this.field('title', {boost: 10})
    	this.field('description')
	    this.ref('id')
	})
	
Next we monitor our data and update the index accordingly:

    var ref = new Firebase("YOUR URL");
    ref.child("topics").on("child_added", function(snapshot) {
            ref.child("topics").child(snapshot.key()).on("value", function(snapshot) {
                    index.update({
                            id: snapshot.val().id,
                            title: snapshot.val().title,
                            description: snapshot.val().desc
                    });
                    console.log("updated index for " + snapshot.val().id);
            });
    });
    ref.child("topics").on("child_removed", function(snapshot) {
            index.remove({id:snapshot.key()});
            console.log("removed index for " + snapshot.key());
    });

Basically we monitor each `topic` for changes and monitor deletes.
If something changes we update our index.  
  
Next we create a webserver and handle the search requests. I'm using the get parameter `q`. So a search request from a client would look like: `http://supersearch.com?q=Team`.
  
    http.createServer(function (req, res) {
            var url_parts = url.parse(req.url, true);
            var query = url_parts.query.q;
            console.log("search query: "+query)
            res.writeHead(200, {'Content-Type': 'application/json'});
            res.end(JSON.stringify(index.search(query)));
    }).listen(1337, '127.0.0.1');
    
---
And that's already all that's needed.  
Here's the whole file: https://gist.github.com/D-32/2dfde2bcf6f149a9ec09