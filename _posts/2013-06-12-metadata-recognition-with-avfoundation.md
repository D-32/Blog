---
layout: post
title:  "Metadata Recognition with AVFoundation"
date:   2013-06-12 00:00:00
categories: iOS
---
iOS 6 let’s you detect faces on a AVCapture preview. In iOS 7 barcodes and qr-codes have been added and work in the same way.    

ViewController.h:

	#import <UIKit/UIKit.h>
	#import <AVFoundation/AVFoundation.h>
	 
	@interface ViewController : UIViewController <AVCaptureMetadataOutputObjectsDelegate>
	 
	@end
	
ViewController.m:

	#import "ViewController.h"
	#import <AudioToolbox/AudioServices.h>
	 
	@implementation ViewController
	 
	- (void)viewDidAppear:(BOOL)animated {
	 
	    [super viewDidAppear:animated];
	     
	    NSError* error;
	     
	    AVCaptureSession* session = [[AVCaptureSession alloc] init];
	    [session setSessionPreset:AVCaptureSessionPresetHigh];
	     
	    AVCaptureDevice* captureDevice = [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeVideo];
	 
	    AVCaptureDeviceInput* deviceInput = [AVCaptureDeviceInput deviceInputWithDevice:captureDevice error:&error];
	    if ([session canAddInput:deviceInput]) {
	        [session addInput:deviceInput];
	    }
	     
	    AVCaptureVideoPreviewLayer *previewLayer = [[AVCaptureVideoPreviewLayer alloc] initWithSession:session];
	    [previewLayer setVideoGravity:AVLayerVideoGravityResizeAspectFill];
	     
	    CALayer *rootLayer = [[self view] layer];
	    [rootLayer setMasksToBounds:YES];
	    [previewLayer setFrame:CGRectMake(-70, 0, rootLayer.bounds.size.height, rootLayer.bounds.size.height)];
	    [rootLayer insertSublayer:previewLayer atIndex:0];
	     
	    [session startRunning];
	     
	    AVCaptureMetadataOutput* output = [[AVCaptureMetadataOutput alloc] init];
	    [output setMetadataObjectsDelegate:self queue:dispatch_get_main_queue()];
	    if ([session canAddOutput:output]) {
	        [session addOutput:output];
	    }
	     
	    // add the desired metadata types here:
	    output.metadataObjectTypes = @[ AVMetadataObjectTypeFace ];
	}
	 
	- (void)captureOutput:(AVCaptureOutput *)captureOutput didOutputMetadataObjects:(NSArray *)metadataObjects fromConnection:(AVCaptureConnection *)connection {
	     
	    // handle output
	}
 
	@end