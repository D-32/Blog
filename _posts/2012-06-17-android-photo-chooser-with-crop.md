---
layout: post
title:  "Android photo chooser with crop"
date:   2012-06-17 00:00:00
categories: Android
---
Launch the photo chooser like this:

	Intent intent = new Intent(Intent.ACTION_PICK,  android.provider.MediaStore.Images.Media.INTERNAL_CONTENT_URI);
	intent.setType("image/*");
	intent.putExtra("crop", "true");
	intent.putExtra("scale", true);
	intent.putExtra("outputX", PHOTO_WIDTH);
	intent.putExtra("outputY", PHOTO_HEIGHT);
	intent.putExtra("aspectX", 1);
	intent.putExtra("aspectY", 1);
	intent.putExtra("return-data", true);
	startActivityForResult(intent, 1);
	
`PHOTO_WIDTH` and `PHOTO_HEIGHT` is the width and height the output photo will have. Android will do all the resizing for you.
You can also set the aspect-ratio, in this example it’s 1:1.

After the photo has been chosen and cropped the user will return to your application:

	@Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
	    if (resultCode != RESULT_OK) {
	        return;
	    }
	 
	    if (requestCode == 1) {
	        final Bundle extras = data.getExtras();
	 
	        if (extras != null) {
	            Bitmap photo = extras.getParcelable("data");
	        }
	    }
	}
	
And now you have your photo as Bitmap :)