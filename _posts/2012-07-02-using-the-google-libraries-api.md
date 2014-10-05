---
layout: post
title:  "Using the Google Libraries API"
date:   2012-07-02 00:00:00
categories: Web
---
jQuery, Dojo and other JavaScript libaries are very popular in web development. Many just download the library and include it into their html file like this:

	<script type="text/javascript" src="jQuery.min.js"></script>

So for every site that uses jQuery the client browser will have to download a copy of the library and cache each one separately. Google offers free hosting of popular JavaScripts. Just replace your script tag with this one:
	
	<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js" ></script>


So what’s the point of this? First of all, if a visitor has already been on a site that uses the Google Libraries API he won’t have to download the jQuery file again. And because many big sites use it, chances are high that he’ll already have it cached.
A second reason to use the Service is Google’s CDN. Google will provide a server that is closist for your visitor. This usually means shorter download times.
—
But what happens if Google is down or the Google domain is blocked in the visitors country? In case this happens it’s good to have a copy of the library on your own server. The fallback-option can be implemented like this:

	<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
	<script type="text/javascript">
	    if (typeof jQuery == 'undefined'){
	        document.write(unescape("%3Cscript src='/path/to/your/jquery'  type='text/javascript'%3E%3C/script%3E"));
	    }
	</script>