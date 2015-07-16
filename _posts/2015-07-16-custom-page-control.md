---
layout: post
title:  "Custom Page Control For UIPageViewController"
date:   2015-07-16 13:00:00
categories: iOS
---
I've found that the delegate methods from UIPageViewController don't give me enough information to create a perfect page indicator control. `pageViewController:willTransitionToViewControllers:` is too early and `pageViewController:didFinishAnimating:previousViewControllers:transitionCompleted:` gets called after the animation is finished which is too late to update a page indicator.  

The best time to update a page indicator is either fluently during the transition or switch to the next / previous "dot" as soon as 50% is reached. This can be accomplished by accessing the UIPageViewControllers scroll view and becoming its delegate. Hacky, yes, so be cautious.  
  
	for (UIView *v in pageViewController.view.subviews) {
		if ([v isKindOfClass:[UIScrollView class]]) {
			((UIScrollView *)v).delegate = self;
		}
	}
	
-

    #pragma mark - UIPageViewControllerDelegate
    - (void)pageViewController:(UIPageViewController *)pageViewController didFinishAnimating:(BOOL)finished previousViewControllers:(NSArray *)previousViewControllers transitionCompleted:(BOOL)completed {
    	NSInteger index = [_viewControllers indexOfObject:pageViewController.viewControllers.firstObject];
    	[_pageIndicatorView setCurrentIndex:index];
    	_currentIndex = index;
    }

    #pragma mark - UIScrollViewDelegate
    - (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    	CGFloat offset = scrollView.contentOffset.x - self.view.frame.size.width;
    	if (offset > 0 && _currentIndex < _viewControllers.count) {
    		// forward
    		if (offset > self.view.frame.size.width / 2) {
    			[_pageIndicatorView setCurrentIndex:_currentIndex+1];
    		} else {
    			[_pageIndicatorView setCurrentIndex:_currentIndex];
    		}
    	} else if (offset < 0 && _currentIndex > 0) {
    		// backward
    		if (offset < -(self.view.frame.size.width / 2)) {
    			[_pageIndicatorView setCurrentIndex:_currentIndex-1];
    		} else {
    			[_pageIndicatorView setCurrentIndex:_currentIndex];
    		}
    	}
    }
    
_pageIndicatorView is my custom pag control, nothing fancy.  
As you can see we additionally use the `didFinishAnimating` delegate method to handle the falling back whne the user lets go during a transition.