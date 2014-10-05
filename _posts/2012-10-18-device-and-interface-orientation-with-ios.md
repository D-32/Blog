---
layout: post
title:  "Device and Interface Orientation with iOS"
date:   2012-10-18 00:00:00
categories: iOS
---
First, you have to know the difference between UIDeviceOrientation and UIInterfaceOrientation.
The device orientation is the orientation the device is currently at. We can get the device orientation with this call:
	
	[[UIDevice currentDevice] orientation]

This will return one of the following states:

	UIDeviceOrientationUnknown
	UIDeviceOrientationPortrait
	UIDeviceOrientationPortraitUpsideDown
	UIDeviceOrientationLandscapeLeft
	UIDeviceOrientationLandscapeRight
	UIDeviceOrientationFaceUp
	UIDeviceOrientationFaceDown

As you can see there are two states FaceUp and FaceDown. One of these will be returned when the device lays flat on a table. So when this is the case, we don’t know how to layout our UI.

The trick is to get the UIInterfaceOrientation of the status bar. As we want our GUI to be at the same orientation as the status bar. To get the UIInterfaceOrientation from the statusbar we use this code:

	[[UIApplication sharedApplication] statusBarOrientation]

A small example how to use it:

	UIInterfaceOrientation orientation = [UIApplication sharedApplication].statusBarOrientation;
	if(orientation == UIInterfaceOrientationLandscapeLeft) {
	    // orientation is landscape left
	} else if(orientation == UIInterfaceOrientationLandscapeRight) {
	    // orientation is landscape right
	} else if(orientation == UIInterfaceOrientationMaskPortrait) {
	    // orientation is portrait
	} else if(orientation == UIInterfaceOrientationMaskPortraitUpsideDown) {
	    // orientation is portrait upsidedown
	}

If you just want to know if the orientation is portrait or landscape you can use these two methods:

	UIInterfaceOrientationIsLandscape(orientation)
	UIInterfaceOrientationIsPortrait(orientation)