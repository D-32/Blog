---
layout: post
title:  "Positioning a footer at the bottom"
date:   2012-07-06 00:00:00
categories: Web
---
The goal is to have a footer stick to the bottom of the browser (making the website extend to the bottom) or, if the there’s no free space stick to the end of the website.

	<div id="wrapper">
	    <div id="container">
	        your content
	         
	        <div id="footer">
	            your footer
	        </div>
	    </div>
	</div>

–
	
	html{
	    height: 100%;
	}
	body{
	    padding: 0;
	    margin: 0;
	    height: 100%;
	    font-family: Arial, Helvetica, sans-serif;
	    font-size: 12px;
	    background-color: #999;
	}
	 
	#wrapper{
	    width: 900px;
	    margin: auto;
	    background-color:#fff;
	    height: auto !important;
	    margin: auto;
	    min-height: 100%;
	    overflow: hidden;
	    position: relative;
	}
	 
	#container{
	    height: 100%;
	    min-height: 100%;
	    width: 900px;
	    margin:auto;
	    padding-bottom: 80px;
	}
	 
	#footer{
	    width: 900px;
	    height: 80px;
	    position: absolute;
	    bottom: 0;
	    color: #eee;
	    background-color:#333;
	}