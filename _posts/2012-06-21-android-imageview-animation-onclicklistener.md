---
layout: post
title:  "Android ImageView Animation + OnClickListener"
date:   2012-06-21 00:00:00
categories: Android
---
When using the TranslateAnimation on an ImageView onClickEvents don’t work as expected. The Image moves on the screen, but the EventListener is still active in the previous position:

![image]({{ site.baseurl }}/uploads/iv_move.png)

To avoid this ghosting evect we have to move the View after the Animation ended. To detect when the animation ends we have to use our own AnimationListener (inside our Activity-Class):

	private class MyAnimationListener implements AnimationListener{
	 
	    @Override
	    public void onAnimationEnd(Animation animation) {
	        imageView.clearAnimation();
	        LayoutParams lp = new LayoutParams(imageView.getWidth(), imageView.getHeight());
	        lp.setMargins(50, 100, 0, 0);
	        imageView.setLayoutParams(lp);
	    }
	 
	    @Override
	    public void onAnimationRepeat(Animation animation) {
	    }
	 
	    @Override
	    public void onAnimationStart(Animation animation) {
	    }
	     
	}
-
	
	TranslateAnimation animation = new TranslateAnimation(0, 50, 0, 100);
	animation.setDuration(1000);
	animation.setFillAfter(false);
	animation.setAnimationListener(new MyAnimationListener());
	 
	imageView.startAnimation(animation);
	
So the onClickEvent will get fired again at it’s new place :) The animation will now move it even more down, so you might want to save the x and y in a variable, so that in the onAnimationEnd() you move it not to a fix location…