---
layout: post
title:  "Android WebView Content"
date:   2014-01-13 00:00:00
categories: Android
---
Sometimes you want to look at the html that’s currently beeing shown in a webview. Using a `JavascriptInterface` this is easy and works on every Android version. 
  
When using API 17+ you’ll need the `@JavascriptInterface` annotation.

	webView.addJavascriptInterface(new JSInterface(), "HtmlViewer");
	webView.loadUrl("javascript:window.HtmlViewer.printHtml" + "('<html>'+document.getElementsByTagName('html')[0].innerHTML+'</html>');");
	
-

	class JSInterface {
	    @JavascriptInterface
	    public void printHtml(String html) {
	        Log.v("LOG", html);
	    }
	}