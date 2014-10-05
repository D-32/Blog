---
layout: post
title:  "Android GPS-Location"
date:   2012-06-24 00:00:00
categories: Android
---
I wrote this LocationHelper class to get the gps (or wifi) location.

	public class LocationHelper {
	 
	    private MyLocation location;
	 
	    Context context;
	 
	    Timer timer;
	    LocationManager lm;
	    boolean gps_enabled=false;
	    boolean network_enabled=false;
	    ILocation activity;
	 
	    private LocationHelper(){
	    }
	 
	    public LocationHelper(Context context){
	        this();
	 
	        this.context = context;
	        location = new MyLocation();
	    }
	 
	    public LocationHelper(Context context, ILocation activity){
	        this(context);
	 
	        this.activity = activity;
	    }
	 
	    LocationListener locationListenerGps = new LocationListener() {
	        public void onLocationChanged(Location location) {
	            timer.cancel();
	            gotLocation(location);
	            // gps will keep going
	            lm.removeUpdates(locationListenerNetwork);
	        }
	        public void onProviderDisabled(String provider) {}
	        public void onProviderEnabled(String provider) {}
	        public void onStatusChanged(String provider, int status, Bundle extras) {}
	    };
	 
	    LocationListener locationListenerNetwork = new LocationListener() {
	        public void onLocationChanged(Location location) {
	            timer.cancel();
	            gotLocation(location);
	            lm.removeUpdates(this);
	            lm.removeUpdates(locationListenerGps);
	        }
	        public void onProviderDisabled(String provider) {}
	        public void onProviderEnabled(String provider) {}
	        public void onStatusChanged(String provider, int status, Bundle extras) {}
	    };
	 
	    class GetLastLocation extends TimerTask {
	        @Override
	        public void run() {
	             lm.removeUpdates(locationListenerGps);
	             lm.removeUpdates(locationListenerNetwork);
	 
	             Location net_loc=null, gps_loc=null;
	             if(gps_enabled){
	                 gps_loc=lm.getLastKnownLocation(LocationManager.GPS_PROVIDER);
	             }
	             if(network_enabled){
	                 net_loc=lm.getLastKnownLocation(LocationManager.NETWORK_PROVIDER);
	             }
	 
	             //if there are both values use the latest one
	             if(gps_loc!=null && net_loc!=null){
	                if(gps_loc.getTime() > net_loc.getTime()){
	                     gotLocation(gps_loc);
	                }else{
	                     gotLocation(net_loc);
	                }
	                return;
	             }
	 
	             if(gps_loc!=null){
	                 gotLocation(gps_loc);
	                 return;
	             }
	             if(net_loc!=null){
	                 gotLocation(net_loc);
	                 return;
	             }
	 
	             gotLocation(null);
	        }
	    }
	 
	    private void gotLocation(Location newLocation){
	        if(newLocation != null){
	            if(activity != null){
	                activity.updateDirectionOnObjects(newLocation);
	            }
	 
	            location.setLatitude(newLocation.getLatitude());
	            location.setLongitude(newLocation.getLongitude());
	        }
	    }
	 
	    public MyLocation getLocation() {
	        return location;
	    }
	 
	    public void register() {
	        if(lm==null){
	            lm = (LocationManager) context.getSystemService(Context.LOCATION_SERVICE);
	        }
	 
	        //exceptions will be thrown if provider is not permitted.
	        try{gps_enabled=lm.isProviderEnabled(LocationManager.GPS_PROVIDER);}catch(Exception ex){}
	        try{network_enabled=lm.isProviderEnabled(LocationManager.NETWORK_PROVIDER);}catch(Exception ex){}
	 
	        //don't start listeners if no provider is enabled
	        if(gps_enabled || network_enabled){
	            if(gps_enabled){
	                lm.requestLocationUpdates(LocationManager.GPS_PROVIDER, 12000, 0, locationListenerGps);
	            }
	            if(network_enabled){
	                lm.requestLocationUpdates(LocationManager.NETWORK_PROVIDER, 0, 0, locationListenerNetwork);
	            }
	 
	            timer=new Timer();
	            timer.schedule(new GetLastLocation(), 20000);
	        }
	    }
	 
	    public void unregister() {
	        lm.removeUpdates(locationListenerGps);
	        lm.removeUpdates(locationListenerNetwork);
	    }
	}
	
MyLocation is a simple model class:

	public class MyLocation {
	    private double latitude;
	    private double longitude;
	 
	    public double getLatitude() {
	        return latitude;
	    }
	    public void setLatitude(double latitude) {
	        this.latitude = latitude;
	    }
	    public double getLongitude() {
	        return longitude;
	    }
	    public void setLongitude(double longitude) {
	        this.longitude = longitude;
	    }
	}
	
ILocation is a interface that can be implemented if you want a feedback when the location is found:

	public interface ILocation {
	    public void updateDirectionOnObjects(Location location);
	}
	
To use it you will have to create a LocationHelper object:

	private LocationHelper locationHelper;
	
In the onCreate-Method:

	locationHelper = new LocationHelper(this);
	//If you want to use the feedback-method:
	//locationHelper = new LocationHelper(this,this);
	//Note: you will have to implement ILocation for this to work!
	
In the onResume-Method (not onCreate!) register the listener:

	locationHelper.register();

In the onPause-Method unregister the listener:

	locationHelper.unregister();

If you didn’t implement ILocation you can get the current Location with following call:

	locationHelper.getLocation();