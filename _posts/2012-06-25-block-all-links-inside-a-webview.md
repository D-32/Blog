---
layout: post
title:  "Block all links inside a WebView"
date:   2012-06-25 00:00:00
categories: iOS
---
To controll what happens when a user clicks on a link inside a WebView you will have to implement your own WebViewClient and override the shouldOverrideUrlLoading() method.

	public class CustomWebViewClient extends WebViewClient {
	    @Override
	    public boolean shouldOverrideUrlLoading(WebView view, String url) {
	        // here you can check the url (whitelist / blacklist)
	        return true; // will NOT load the link, use "return false;" to allow it to load
	    }
	}
	
Using your new WebViewClient is easy:

	webview.setWebViewClient(new CustomWebViewClient());