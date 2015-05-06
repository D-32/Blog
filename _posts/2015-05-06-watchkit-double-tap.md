---
layout: post
title:  "WatchKit Double Tap"
date:   2015-05-06 18:00:00
categories: iOS
---
Sadly there's no double tap gesture in the current WatchKit version. It is possible though to manually detect double taps.  
This isn't possible with a button because the system doesn't send all tap events if one is too fast. But with a table we get all the taps unfiltered and can now messure the time between taps and detect double taps.  
  
Just create a table with one row and put whatever you want tappable inside that row.  
  
	@implementation InterfaceController {
    	NSDate* _lastTouch;
	}

	
    - (void)table:(WKInterfaceTable *)table didSelectRowAtIndex:(NSInteger)rowIndex {
        OverlayRowController *rowController = [table rowControllerAtIndex:rowIndex];
        if (_lastTouch && [[NSDate date] timeIntervalSinceDate:_lastTouch] < 0.4) {
            // double tap
        }
        _lastTouch = [NSDate date];
    }


Simple as that. We store the time for each tap and if the last tap was < 0.4s ago we consider it a double tap.