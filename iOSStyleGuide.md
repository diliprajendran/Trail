iOS Developer Style Guide
=========================

	Source : https://github.com/troyharris/Complete-iOS-StyleGuide
	Source : https://github.com/NYTimes/objective-c-style-guide
	
Here are some of the documents from Apple that informed the style guide. If something isn’t mentioned here, it’s probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)


Table of Contents
-----------------
* [Comments](#comments)
* [Prefixing](#prefixing)
* [Brackets](#brackets)
* [Variables](#variables)
* [Methods](#methods)
* [Properties](#properties)
* [Constants](#constants)
* [Enums and Bitmasks](#enums-and-bitmasks)
* [Literals](#literals)
* [Booleans](#booleans)
* [Foundation Data Types vs C Primative Data Types](#foundation-data-types-vs-c-primative-data-types)
* [Dynamic vs Static Typing](#dynamic-vs-static-typing)
* [CGGeometry Methods](#cggeometry-methods)
* [View Controller Class Names](#view-controller-class-names)
* [Class Headers](#class-headers)
* [Implementation](#implementation)
* [Subclassing, Categories and Extensions](#subclassing-categories-and-extensions)
* [Dot Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Init & Dealloc](#init-and-dealloc)
* [CGRect Functions](#cgrect-functions)
* [Private Properties](#private-properties)
* [Image Naming](#image-naming)
* [Singletons](#singletons)
* [Imports](#imports)
* [Project Organization](#project-organization)

<a name="comments"></a>
Comments
--------
Objective-C should be coded in a way that is very descriptive. If done right, comments can be kept to a minimum. Only comment when you need to explain *why* the code is written the way it is.

Comments should be written on their own line and have a blank line between itself and the code above it. Block comments should be avoided. 

**Example**

```objc
int carCount = 0;

//This is a comment for the line below
carCount++;
```

<a name="prefixing"></a>
Prefixing
---------
Every project should have a three character, capitolized prefix to be used in class names, static variables, `#define`s, etc. Each project should have its own prefix rather than using a company prefix or a person's initials.

Core data names and NSManagedObject subclasses should not use the prefix.

<a name="brackets"></a>
Brackets
--------
Opening brackets should be at the end of line of the statement they are bracketing. Closing brackets should be on their own lines. Conditionals should always have brackets.

**Example**

```objc
//Good
if (number == 1) {
	[self doThings];
}

//Bad
if (number == 1)
{
	[self doThings];
}

//Bad
if (number == 1)
	[self doThings];

//Bad
if (number == 1) [self doThings];

//Bad
if (number == 1) { [self doThings]; }
```

An exception to the rule is `else` statements. These should be on the same line as the closing bracket. This serves as a visual indicator that the `else` block is related to the `if` block.

**Example**

```objc
//Good
if (number == 1) {
	[self doSomething];
} else {
	[self doSomethingElse];
}

//Good
if (number == 1) {
	[self doSomething];
} else if (number == 2) {
	[self doSomethingElse];
}

//Bad
if (number == 1) {
	[self doSomething];
}
else {
	[self doSomethingElse];
}
```

<a name="variables"></a>
Variables
---------
Variables should be named descriptively and avoid use of acronyms. Pointer asterisk should be with the variable name. If the variable is of a common type, avoid putting that in the name. Variable names should start with a lowercase letter and be camelcased.

**Examples**

```objc
//Good
NSString *carModel = @"Mustang";
CGFloat noteViewWidth = 30;


//Bad
NSString * carModelString = @"Mustang";
CGFloat nwidth = 30;
```

There are times when it is acceptible to add the type in the variable name when omitting it may lead to confusion. For example:

```objc
NSDate *currentDate = [NSDate date];
//'String' is appended to the variable name to avoid confusion that it might be an NSDate object.
NSString *currentDateString = [XYZDateFormatter stringFromDate:currentDate];
```

<a name="methods"></a>
Methods
-------
Methods should be formatted as so (notice the space after the scope symbol)

```objc
//Instance method
- (void)buildJSONRequestFromString:(NSString *)request withTag:(int)tag;
 
//Class method
+ (XYZString *)uppercaseStringFromString:(NSString *)string;
```

Method names should be as descriptive as possible. Avoid using names that start off with *set* or *get* unless it is a setter or getter method. Also avoid the prefix *is* unless it returns a BOOL.

If possible, methods should return its last line of code:

```objc
//Good
- (NSString *)stringFromInt:(int)number {
	return [NSString stringWithFormat:@"%d", number];
}

//Bad
- (NSString *)stringFromInt:(int)number {
	NSString *numberString = [NSString stringWithFormat:@"%d", number];
	return numberString;
}
```

<a name="properties"></a>
Properties
----------
Properties should be defined for all instance variables whenever possible. Only declare properties in the header file when outside classes need access. Avoid using @synthesize unless needed to make a public readonly property privately writable. Avoid accessing instance variables directly unless in initializer methods, dealloc, or getter and setter methods.

**Examples**

```objc
//Good
self.notes = [[NSArray alloc] init];

//Bad
_notes = [[NSArray alloc] init];

//Good
@interface XYZNoteView : XYZSuperView

@property (nonatomic, strong) NSMutableArray *notes;

@end

//Bad
@interface XYZNoteView : XYZSuperView {
	NSArray _notes;
}

@property (nonatomic, strong) NSMutableArray *notes;

@end
```

Properties should always be accessed using dot notation (`car.model` rather than `[car model] or [car setModel]`).

<a name="constants"></a>
Constants
---------
Instance constants should start with a lowercase `k`, followed by the project prefix, followed by a descriptive name.

**Example**

```objc
static NSString * const kXYZProjectCellID = @"ProjectCell";
static CGFloat const kXYZMenuTopMargin = 80;
```

Use of `#define` to set constants should be avoided.

<a name="enums-and-bitmasks"></a>
Enums and bitmasks
-----------------
An `enum` should be defined using NS_ENUM and bitmasks should be defined using NS_OPTIONS. The enum or bitmask should be named using the project prefix

**Examples**

```objc
// enum
typedef NS_ENUM(NSInteger, XYZCarType) {
    XYZCarTypeVan,
    XYZCarTypeTruck,
    XYZCarTypeSedan,
    XYZCarTypeHybrid
};

// bitmask
typedef NS_OPTIONS(NSInteger, XYZCollisionCategory) {
    XYZCollisionCategoryCar,
    XYZCollisionCategoryWall,
    XYZCollisionCategoryBuilding,
    XYZCollisionCategoryPerson
};
```

<a name="literals"></a>
Literals
--------
`NSString`, `NSNumber`, `NSArray`, and `NSDictionary` literals should be used whenever possible.

```objc
NSString *name = @"John Doe";
NSNumber *maxSpeed = @160;
NSNumber *halfSpeed = @(maxSpeed / 2);
NSArray *colorNames = @[@"blue", @"red", @"green"];
NSDictionary *driver = @{@"name": name, @"maxSpeed": maxSpeed, @"nickName": @"crash"};
```

<a name="booleans"></a>
Booleans
--------
`BOOL` should always be used over `bool`. BOOLs should always be compared as so:

```objc
//Good
if (person.isRunning) {
}
if (!person.isRunning) {
}

//Bad
if (person.isRunning == nil) {
}
if (person.isRunning == NO) {
}
if (person.isRunning == YES) {
}
```

<a name="foundation-data-types-vs-c-primative-data-types"></a>
Foundation Data Types vs C Primative Data Types
-----------------------------------------------
Use `NSInteger`, `NSUInteger` and 'CGFloat' instead of `int`, `long`, `unsigned int`, `unsigned long`, `float`, or `double`. This makes your code 64bit safe.

Dynamic vs Static Typing
------------------------
Static typing should nearly always be used unless dynamic typing is absolutely necessary or greatly simplifies the code.

**Example**

```objc
//Good
NSString *name = @"Waldo";

//Bad (unless you have a very good reason to do it)
id name = @"Waldo";
```

<a name="cggeometry-methods"></a>
CGGeometry Methods
------------------
Use CGGeometry methods (`CGRectGetMinX`, `CGRectGetHeight`, etc) rather than grabbing frame properties manually. 

**Example**

```objc
//Good
CGFloat carWidth = CGRectGetWidth(car.frame);

//Bad
CGFloat carWidth = car.frame.size.width;
```

Also, consider the other awesome abilities of CGGeometry when manupulating `CGRect`s in any way. NSHipster has [a good article](http://nshipster.com/cggeometry/) about this.

<a name="view-controller-class-names"></a>
View Controller Class Names
-----------
View controller names follow standard class naming conventions (capitolized project prefix, camelcased descriptive name) but are also suffixed with either `ViewController`, `TableViewController` or `CollectionViewController`, depending on type. Avoid shortening to `VC`, `TVC`, or `CVC`.

<a name="class-headers"></a>
Class Headers
------------
Keep headers as simple as possible. Only include properties and methods that other classes need access to. Consider making some properties readonly if they shouldn't be changed by an outside class. 

Header files should be organized as so:

```objc
//Import statements first. Import in this order: superclass, frameworks, local classes
#import "XYZSuperView.h"
#import <UIKit/UIKit.h>
#import "XYZCar.h"

//Classes should be forward declared whenever possible. Just make sure you #import in the .m file.
@Class XYZDriver;
@Class XYZPassenger;

//Any protocol definitions next. Protocol should always include the class name in its own name.
@protocol XYZNotePopoverDelegate <NSObject>

@optional
- (void)closedPopover:(XYZNotePopover *)notePopover;

@end

//Interface
@interface XYZNoteView : XYZSuperView <XYZCarDelegate>

//Properties - Only properties that need to be included publicly. Otherwise, define in the .m file
@property (nonatomic, strong) XYZDriver *driver;

//Even create properties for C primitive types
@property int numberOfCrashes;

//Instance methods
- (NSString *)driverNameFromDriver:(Driver *)driver;

//Class methods
+ (XYZNoteView *)noteViewFromPassenger:(Passenger *)passenger;
```

<a name="implementation"></a>
Implementation
--------------
Implementation files should be organized in a useful way.

Methods should be organized in the following order: 

* Init and dealloc methods
* Super class methods being overwritten
* Custom getters and setters
* Delegate methods
* Private instance methods
* Public instance methods (methods defined in the header file)
* Public class methods (methods defined in the header file)

Each of these sections should be seperated with a `#pragma mark` as so:

```objc
#pragma mark - UIViewController methods
```

Sections can be further broken down into subsections by using `#pragma mark` without the hyphen.

<a name="protocols"></a>
Protocols
---------
Protocol methods go under `@optional` unless a delegation absolutely won't function without the protocol method. It is good practice to allow the object of the protocol to be passed to the delegate. This will allow multiple delegations on the delegate object.

**Example**

```objc
//We must call @class first on our own class because the @protocol comes before the @interface
@class XYZNotePopover
@protocol XYZNotePopoverDelegate <NSObject>

@optional

//We pass the XYZNotePopover to the delegate so it can determine who called it.
- (void)closedPopover:(XYZNotePopover *)notePopover;

@end
```

<a name="subclassing-categories-and-extensions"></a>
Subclassing, Categories and Extensions
--------------------------------------
When possible, it is best to use categories rather than subclassing. This will encourage composition over inheritance. Categories should serve one function. For example, you might define `UIColor+XYZProjectColor` to house methods for retrieving common colors used in your project. If you have a need for custom color blending methods, create a second category named `UIColor+XYZColorBlend`.

Extensions are another way to avoid subclassing. Use these when you simply need to add additional properties to a custom class. Private properties should be declared in an anonymous extension.

When subclassing must be used, name the subclass in a way that suggests the super class. For example, `XYZMenuView` might be subclassed as `XYZCarMenuView`.

## Dot Notation Syntax

Dot notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```
## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

**For example:**
```objc
if (user.isHappy) {
    // Do something
}
else {
    // Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization.
* Whitespace within methods should be used to separate functionality (though often this can indicate an opportunity to split the method into several, smaller methods). In methods with long or verbose names, a single line of whitespace may be used to provide visual separation before the method’s body.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent [errors](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line “inside” the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**For example:**
```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

### Ternary Operator

The ternary operator, `?` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into named variables.

**For example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error Handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**For example:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Not:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## init and dealloc

`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

`init` methods should be structured like this:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initializer
    if (self) {
        // Custom initialization
    }

    return self;
}
```
## `CGRect` Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**For example:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```
## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class.

**For example:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose should be grouped in respective groups in an Images folder or Asset Catalog.

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[[self class] alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes frequent crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Imports

If there is more than one import statement, group the statements [together](http://ashfurrow.com/blog/structuring-modern-objective-c). Commenting each group is optional.

Note: For modules use the [@import](http://clang.llvm.org/docs/Modules.html#using-modules) syntax.

```objc
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```


<a name="project-organization"></a>
Project Organization
--------------------
Files should be organized in directories that match their XCode groups.

MVC seperation should be applied to directories and groups: data model classes, view controllers and custom views should be seperate directories. 

Other groups and directories should be created for other assets and classes in some sort of logical scheme based on the project. The root directory should only contain `AppDelegate.h` and `AppDelegate.m`

Leave things that XCode creates automatically (such as `main.m` and `Prefix.pch`) in their original location and their original name whenever possible.