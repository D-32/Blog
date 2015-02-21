---
layout: post
title:  "Self Storing Model"
date:   2015-02-21 22:30:00
categories: iOS
---
I wanted a simple way to persist my model changes without having any boilerplate code in the model classes.
With this solution any model object just has to inherit from `Model`, that's it.  

Every model object will observe its own properties and when a change occurs it will persisted. In this example everything just gets saved to disk. Obviously for bigger amounts of data the storage part would have to be done better.

#### For this to work, please add [AutoCoding](https://github.com/nicklockwood/AutoCoding) to your project.

Model.m:

	@implementation Model {
	    NSObject *_temp;
	}
	
	- (instancetype)init {
	    if (self = [super init]) {
	        [self modelSetup];
	    }
	    return self;
	}
	
	- (id)initWithCoder:(NSCoder *)aDecoder {
	    if (self = [super initWithCoder:aDecoder]) {
	        [self modelSetup];
	    }
	    return self;
	}
	
	- (void)modelSetup {
	    NSDictionary *dict = [[self class] codableProperties];
	    for (NSString *property in dict.allKeys) {
	        [self addObserver:self forKeyPath:property options:NSKeyValueObservingOptionPrior context:nil];
	    }
	}
	
	- (void)dealloc {
	    NSDictionary *dict = [[self class] codableProperties];
	    for (NSString *property in dict.allKeys) {
	        [self removeObserver:self forKeyPath:property];
	    }
	}
	
	- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context {
	    id value = [object valueForKey:keyPath];
	    if ([change[@"notificationIsPrior"] isEqualToNumber:@1]) {
	        _temp = value;
	    } else {
	        if (![_temp isEqual:value]) {
	#warning TODO
				// call some singelton here to save your data
	        }
	    }
	}
	
	@end
	
