---
layout: post
title:  "Android Remote Gallery"
date:   2014-01-26 00:00:00
categories: Android
---
Simple Activity to display some images from a server.
The images are first downloaded and then presented inside a ViewPager. Works for all Android versions.

	public class GalleryActivity extends Activity {
	     
	    @Override
	    public void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.gallery);
	 
	        final List<String> urls = getUrls(); // get your urls here
	        final List<Bitmap> images = new ArrayList<Bitmap>();
	         
	        final ViewPager viewPager = (ViewPager)findViewById(R.id.pager);
	        viewPager.setAdapter(new ImagesAdapter(images));
	        final ProgressDialog dialog = new ProgressDialog(this);
	        dialog.setMessage("Images are beeing loaded...");
	        dialog.show();
	         
	        final Handler downloadFinished = new Handler() {
	            @Override
	            public void handleMessage(Message msg) {
	                super.handleMessage(msg);
	                dialog.hide();
	                viewPager.getAdapter().notifyDataSetChanged();
	            }
	        };
	         
	        new Thread() {
	            public void run() {
	                for (String urlString : urls) {
	                    try {
	                        URL url = new URL(urlString);
	                        URLConnection conn = url.openConnection();
	                        conn.connect();
	                        InputStream is = conn.getInputStream();
	                        BufferedInputStream bis = new BufferedInputStream(is);
	                        images.add(BitmapFactory.decodeStream(bis));
	                        bis.close();
	                        is.close();
	                    } catch (MalformedURLException e) {
	                        dialog.hide();
	                        e.printStackTrace();
	                    } catch (IOException e) {
	                        dialog.hide();
	                        e.printStackTrace();
	                    }
	                }
	                downloadFinished.sendEmptyMessage(Thread.NORM_PRIORITY);
	            };
	        }.start();
	    }
	     
	    class ImagesAdapter extends PagerAdapter {
	         
	        private List<Bitmap> _images;
	         
	        public ImagesAdapter(List<Bitmap> images) {
	            _images = images;
	        }
	         
	        @Override
	        public void startUpdate(View arg0) {
	        }
	         
	        @Override
	        public Parcelable saveState() {
	            return null;
	        }
	         
	        @Override
	        public void restoreState(Parcelable arg0, ClassLoader arg1) {
	        }
	         
	        @Override
	        public boolean isViewFromObject(View view, Object object) {
	            return view == ((RelativeLayout) object);
	        }
	         
	        @Override
	        public Object instantiateItem(View viewPager, int position) {
	            LayoutInflater inflater = (LayoutInflater)GalleryActivity.this.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
	            View container = inflater.inflate(R.layout.fullscreen_image, (ViewGroup)viewPager, false);
	            ImageView image = (ImageView) container.findViewById(R.id.imgDisplay);
	            image.setImageBitmap(_images.get(position));
	            ((ViewPager) viewPager).addView(container);
	       
	            return container;
	        }
	         
	        @Override
	        public int getCount() {
	            return _images.size();
	        }
	         
	        @Override
	        public void finishUpdate(View arg0) {
	        }
	         
	        @Override
	        public void destroyItem(View container, int position, Object object) {
	            ((ViewPager) container).removeView((RelativeLayout) object);
	        }
	    }
	}
	
gallery.xml:

	...
	<android.support.v4.view.ViewPager
	        android:id="@+id/pager"
	        android:layout_width="fill_parent"
	        android:layout_height="fill_parent" />
	...
	
fullscreen_image.xml:
	
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent" >
	  
	    <ImageView
	        android:id="@+id/imgDisplay"
	        android:layout_width="fill_parent"
	        android:layout_height="fill_parent"
	        android:scaleType="fitCenter" />
	  
	</RelativeLayout>
