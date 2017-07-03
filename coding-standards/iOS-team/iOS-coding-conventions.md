# Objective-C Coding Convention and Best Practices

Hầu hết những nguyên tắc được trình bày dưới đây đều phù hợp với tài liệu của Apple cùng với những ví dụ tốt nhất được cộng đồng các lập trình viên chấp nhận. Tài liệu này chủ yếu phục vụ cho lập trình viên iOS nhưng cũng áp dụng tốt cho lập trình viên Mac.

## Usage

Tất cả các dự án iOS và Mac gần đây đều được áp dụng cách mã hoá này, các dự án về trước cũng có thể chuyển đổi sang nhưng nói chung là không nên.

## Warnings

Các cảnh báo (warnings) trong code của bạn nói chung nên để cố định. Trong 1 số trường hợp rất khó để loại bỏ 1 cảnh báo nhưng các lập trình viên biết rõ rằng các cảnh báo đó không ảnh hưởng gì nhiều đến chương trình của họ. Trong trường hợp dưới đây có thể loại bỏ 1 cảnh báo với từ khoá #pragma

```objective-c
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Warc-performSelector-leaks"

// Code that generates a warning but really shouldn't
// I deactivated this warning because ...
// ...

#pragma clang diagnostic pop
```

Có thể dùng cách khác để loiaj bỏ các cảnh báo. Bạn vào Xcode chọn phần Target - Build Phases - Compile Sources sau đó set cờ '-w' cho phần Compiler Flag.

## TODOs

Từ khoá TODO dùng để mô tả ngắn gọn về những gì có thể làm , tham khảo ví dụ dưới đây:

```objective-c
// TODO: Fix the Bug XYZ that happens when the user clicks on the second button while the App is loading
```

Đoạn script dưới đây bạn ghi vào file Target - Build Phases - Add Build Phase - Run Script để tạo 1 cảnh báo cho riêng mình:

```bash
KEYWORDS="TODO:|FIXME:|\?\?\?:|\!\!\!:"

if [ $CONFIGURATION != "Debug" ]; then
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($KEYWORDS).*\$" | perl -p -e "s/($KEYWORDS)/ warning: \$1/"
fi
```

## Comments

`//` dùng để comment 1 dòng. `/**/` dùng để comment nhiều dòng.

Xem thêm tại: http://www.gentlebytes.com/home/appledocapp/

## Operators

```objective-c
NSString *foo = @"bar";
NSInteger answer = 42;
answer += 9;
answer++;
answer = 40 + 2;
```

Các toán hạng nên cách nhau ra bởi dấu space.


## Types

`NSInteger` và `NSUInteger` nên được sử dụng thay vì `int` hay `long` là vì chúng mã hoá 64 bit an toàn hơn. Cũng vì vậy mà  `CGFloat` được sử dụng nhiều hơn so với `float`. Đặc điểm này chứng minh mã tương lai cho nền tảng 64 - bit.

Tất cả các kiểu dữ liệu của Apple nên được sử dụng thay vì kiểu nguyên thuỷ. Ví dụ , nếu bạn đang làm việc với  time intervals, sử dụng `NSTimeInterval` thay vì `double` mặc dù nó là như nhau. Nó lam dòng code của bạn rõ ràng dễ hiểu hơn.

## Spacing
1 dòng code dài nên được chia thành nhiều dòng code nhỏ cho dễ đọc hơn.

```objective-c
// long method name calling convention
color = [NSColor colorWithCalibratedHue:0.10
saturation:0.82
brightness:0.89
alpha:1.00];
```

## Methods

```objective-c
- (void)someMethod {
// Do stuff
}


- (NSString *)stringByReplacingOccurrencesOfString:(NSString *)target withString:(NSString *)replacement
{ // ok here too
return nil;
}
```

Luôn luôn có 1 space giữa `-` hoặc `+` và kiểu giá trị trả về (trong trường hợp này là `(void)`) và không bao giờ có space giữa kiểu trả về và tên hàm.

Không bao giờ có space trước và sau dấu chấm phẩy. Nếu tham số truyền vào là con trỏ, thì luôn có space giữa class và dấu `*`.

Không được bỏ qua kiểu trả về (mặc định kiểu trả về là id cho methods và int cho functions).

Luôn có 1 dấu space giữa method và `{`. `{` có thể trên cùng 1 dòng với method hoặc là xuống dòng.


## Pragma Mark and Implementation Organization

An excerpt of a UIView:

```objective-c

////////////////////////////////////////////////////////////////////////
#pragma mark - Lifecycle
////////////////////////////////////////////////////////////////////////

- (void)dealloc {
// Unregister Notifications, clean up etc.
}


////////////////////////////////////////////////////////////////////////
#pragma mark - UIView
////////////////////////////////////////////////////////////////////////

- (id)layoutSubviews {
[super layoutSubviews];

// Stuff
}


- (void)drawRect:(CGRect)rect {
// Drawing code
}
```

Dùng từ khoá (#pragma mark - Tên group các functions or methods) để group các functions có chung 1 đặc điểm nào đó tiện cho việc quản lí code và tìm kiếm.


## Control Structures

Nên có 1 space sau các từ khoá điều khiển (i.e. `if`, `else`, etc).


### If/Else

```objective-c
if (button.enabled) {
// Stuff
} else if (otherButton.enabled) {
// Other stuff
} else {
// More stuf
}
```

Câu lệnh `else` có thể bắt đầu cùng 1 dòng với câu lệnh `if` hoặc xuống dòng tiếp theo. 

```objective-c
// Comment explaining the conditional
if (something) {
// Do stuff
}

// Comment explaining the alternative
else {
// Do other stuff
}
```
Nếu cần comments xung quanh câu lệnh `if` hoặc `else`,có thể dùng giống ví dụ trên với 1 dòng trống giữa đóng block `}` và dòng comment.

### Ternary Operator

```objective-c
label.frame = (inPseudoEditMode) ? kLabelIndentedRect : kLabelRect;
```

Sử dụng toán tử 3 ngôi cho các câu điều kiện đơn giản.


### Switch

```objective-c
switch (something.state) {
case 0: {
// Something
break;
}

case 1: {
// Something
break;
}

case 2:
case 3: {
// Something
break;
}

default: {
// Something
break;
}
}
```

nên để các lệnh trong các trường hợp(case) trong dấu đóng mở ngoặc. Nếu sử dụng nhiều trường hợp (case) thì nên viết ở các dòng khác nhau. Trường hợp `default` luôn phải ở cuối và chắc chắn phải có.


### For

```objective-c
for (NSInteger i = 0; i < 10; i++) {
// Do something
}


for (NSString *key in dictionary) {
// Do something
}
```

Khi lặp sử dụng số nguyên , nó nên bắ đầu từ `0` và sử dụng `<` thay vì bắt đầu bằng `1` và sử dụng `<=`. Có thể dùng kiểu truy xuất thẳng như ở ví dụ 2 ở trên. Kiểu này thường dễ nhìn và nhanh hơn. Ngoài ra còn có thể sử dụng block nếu câu điều kiện phức tạp hơn.


### While

```objective-c
while (something < somethingElse) {
// Do something
}
```

## Import

Luôn luôn sử dụng `@class` bất cứ khi nào có thể thay vì `#import` giảm thời gian compile.

Tham khảo [Objective-C Programming Guide](http://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ObjectiveC/ObjC.pdf) (Page 38):

> The @class directive minimizes the amount of code seen by the compiler and linker, and is therefore the simplest way to give a forward declaration of a class name. Being simple, it avoids potential problems that may come with importing files that import still other files. For example, if one class declares a statically typed instance variable of another class, and their two interface files import each other, neither class may compile correctly.

### Header Prefix

Nên sử dụng frameworks vào file header prefix. Bởi nếu để ở file này chúng ta sẽ không cần phải import vào các file khác trong project mà vẫn sử dụng được các frameworks này.

Ví dụ, trong file header prefix ta khai báo như sau:

```objective-c
#ifdef __OBJC__
#import <Foundation/Foundation.h>
#import <UIKit/UIKit.h>
#endif
```

## Properties

```objective-c
@property (nonatomic, strong) UIColor *topColor;
@property (nonatomic, assign) CGSize shadowOffset;
@property (nonatomic, unsafe_unretained, readonly) UIActivityIndicatorView *activityIndicator;
@property (nonatomic, assign, getter=isLoading) BOOL loading;
```

Nếu thuộc tính là `nonatomic` thì nó phải được ghi lên đầu tiên. tuỳ chọn tiếp theo luôn là `strong`, `weak`, hoặc `assign` vì nếu bỏ qua nó , sẽ có cảnh báo. `readonly` sẽ là tuỳ chọn tiếp theo nếu nó được qui định. `readwrite` thì không bao giờ được dùng ở file header(file .h). `readwrite` chỉ được sử dụng trong class mở rộng và ghi đè thuộc tính `readonly` trong file header (file .h). `getter` hoặc `setter` thường là ở cuối,`setter` hiếm khi được sử dụng, `getter` thường sử dụng cho biến kiểu BOOL.

Xem ví dụ về cách sử dụng `readwrite` ở phần *Private Methods*.

Sau @synthesize nên sử dụng các tham số là tên thuộc tính có dấu `_` hay còn gọi là iVar để tránh trùng khớp với tên các biến khác. Kể từ khi LLVM 4.0 được sử dụng trên bản Xcode (4.4) thì @synthesize không cần phải sử dụng nữa. LLVM 4.0 tự sinh @synthesize với dấu `_` trước tên các thuộc tính.

```objective-c
@synthesize topColor = _topColor;
```

## Methods
Các phương thức rỗng hoặc chỉ đơn thuần là gọi phương thức super thì nên xoá đi vì nó là code thừa.

## dealloc
Nếu -dealloc được thực hiện(ở chế độ ARC) nên đặt nó ở ngay phương thức init nếu có hoặc cuối class. Nếu không có phương thức -init thì nên đặt nó là phương thức đầu tiên sau class method. Còn ở chế độ non-ARC -dealloc phải được gọi và release tất cả các iVars.

## init
Theo thiết kế thì phương thức init của class luôn được comment lại ở file header. Chúng ta nên ghi đè lại các phương thức này. Ví dụ luôn ghi đè lại phương thức initWithFrame trong lớp con của lớp UIView.

## Private Methods and Properties

MyShoeTier.h

```objective-c
@interface MyShoeTier : NSObject 

// In general no iVar-section

@property (noatomic, strong, readonly) MyShoe *shoe;

...

@end
```

MyShoeTier.m

```objective-c
#import "MyShoeTier.h"

@interface MyShoeTier () {
// Here can go private iVars if there is the need to
}

@property (nonatomic, strong, readwrite) MyShoe *shoe;
@property (nonaomic, strong) NSMutableArray *laces;

- (void)crossLace:(MyLace *)firstLace withLace:(MyLace *)secondLace;

@end

@implementation MyShoeTier

// Here shall go the @synthesize-block
...

@end
```
Private methods nên được tạo ra trong 1 class mở rộng vì đơn giản la từ 1 category đã được đặt tên sẽ không thể được sử dụng nếu nó thêm hoặc chỉnh sửa bất kì 1 thuộc tính nào.

## Naming

Nói chung, mọi thứ nên bắt đầu với 2 ký tự đầu. có thể dùng nhiều ký tự nhưng không khuyến khích.

Nên đặt tên 2 ký tự đầu này đặc trưng cho ứng dụng của bạn. Nếu bạn sử dụng nó ở 1 ứng dụng khác thì nên đặt tên theo công ty của bạn.

1 số ví dụ:

```objective-c
NGLoadingView     // Simple view that can be used in other applications
AppDelegate       // AppDelegate should ALWAYS be named AppDelegate
TVLoadingView     // `NGLoadingView` customized for the application TVthek
```

Classes, Categories, Functions và Protocols nên bắt đầu với chữ hoa. Tên biến , thuộc tính và tên phương thức thì dùng chữ thường.

## Naming Again

Biến,phương thức hoặc class không nên viết tắt, nên viết dài như Apple. Không nên đặt tên mơ hồ, khó hiểu, nên có các tiền tố kết nối câu như `and` và `with` trong tên phương thức.

```objective-c
NGBrowserViewController                             // instead of NGBrowserVC
currentPageIndex                                    // instead of pageIdx or just idx

- (void)updateSegmentTextViewContentSize
- (void)updateBarButtonItemsForInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
```

Phương thức sẽ trả về đối tượng được mô tả bởi tên phương thức , get sẽ không được sử dụng như getter trong các ngôn ngữ khác (ví dụ : Java):

```objective-c
[object thing+condition];
[object thing+input:input];
[object thing+identifer:input];

// Examples:
realPath    = [path stringByExpandingTildeInPath];
fullString  = [string stringByAppendingString:@"Extra Text"];
object      = [array objectAtIndex:3];
```

Nếu bạn muốn biến đổi 1 giá trị này sang giá trị khác có thể tham khảo mẫu dưới đây:

```objective-c
[object adjective+thing];
[object adjective+thing+condition];
[object adjective+thing+input:input];

// Examples:

capitalized = [name capitalizedString];
rate        = [number floatValue];
newString   = [string decomposedStringWithCanonicalMapping];
subarray    = [array subarrayWithRange:segment];
```

## Categories

Categories chủ yếu được sử dụng cho code mà không được implement bởi framework và nên tránh nếu có thể , vì chúng có thể tạo nên sự đụng độ về tên và lỗi lạ. Categories nên được đặt tên như ví dụ duới đây:

```objective-c
@interface UITableView (NGLoading)
@interface UITableView (TVLoading)
```

Các file được đặt tên cho phù hợp:
UITableView+NGLoading.h/.m, UITableView+TVLoading.h/.m


## Global C Functions

Functions sẽ bắt đầu với chữ viết hoa và có cùng kiểu đặt tên như đặt tên class.

```objective-c
NGGetGlobalFormatOfString(@"test");
```

## Extern, Const, and Static

```objective-c
extern NSString *const kNGMyConstant;
extern NSString *myExternString;
static NSString *const kNGMyStaticConstant;
static NSString *staticString;
```

## Notifications

Tên của Notifications nên tuân theo qui luật sau:

Class of Affected Object + Did/Will + Action + "Notification"

Example:

```objective-c
UIApplicationWillResignActiveNotification
```

## Booleans

Biến Boolean có 2 giá trị là YES và NO. YES và NO là hai giá trị được sử dụng thay cho true/false , TRUE/FALSE,0/1,... 
Các kiểu dữ liệu khác muốn sử dụng trong các câu lệnh điều kiện đều phải chuyển vê kiểu BOOL bằng cách sử dụng phép so sánh.Ví dụ:

* if (pointer != nil) thay cho if (pointer)
* if (intValue == 0) thay cho if (!intValue)
* if (strcmp("test",myCString) == 0) thay cho if (!strcmp("test",myCString))
* ...

## Enums

Tên của các giá trị của Enum luôn có những cái đầu là tên của enum , giống như của Apple. Đối với enum kiểu Bitmask-Type thì toán tử shift-left (<<) nên được sử dụng.

```objective-c
enum {
Foo,
Bar
};

typedef enum {
NGLoadingViewStyleLight,
NGLoadingViewStyleDark
} NGLoadingViewStyle;

typedef enum {
NGHUDViewStyleLight = 7,
NGHUDViewStyleDark = 12
} NGHUDViewStyle;

typedef enum {
NGHUDViewTypeSingle = 1 << 0,    // For bitmasks
NGHUDViewTypeDouble = 1 << 1,
NGHUDViewTypeTriple = 1 << 2
} NGHUDViewType;
```

## Objetive C Literals (LLVM 4.0 and up)

Kể từ khi LLVM 4.0 hỗ trợ Xcode 4.4 non-beta giúp ta viết code ngắn gọn dễ đọc hơn.

```objective-c

NSMutableDictionary *status = {@"llvm4IsAwesome" : @YES, @"javaIsAwesome" : @NO};
@status[@"excited"] = @YES; // instead of [status setValue:[NSNumber numberWithBool:YES] forKey:@"excited"];

```

## Dynamic Typing

Sẽ tốt hơn nếu nói cho trinh biên dịch kiểu của biến nếu có thể. Việc sử dụng kiểu `id` là nên tránh, thay vào đó nên sử dụng protocol xác định hoặc tên class.

## Singletons

Nên tránh sử dụng Singletons bất cứ khi nào có thể vì nó tạo ra 1 phụ thuộc global. Tuy nên đôi khi chúng ta vẫn phải sử dụng Singletons nhưng không nên tạo ra 1 `ApplicationManager` chung chung hay đại loại thế mà xử lí rất nhiều chức năng.

## Blocks/GCD

Blocks được sử dụng nhiều và được yêu thích hơn delegate-methods trong hầu hết trường hợp(nhưng không phải tất cả). GCD được sử dụng cho việc xử lí song song 1 cách đơn giản nhưng nếu muốn dừng xử lí song song thì phải sử dụng NSOperation/NSOperationQueue thay GCD.
## Asserts

Asserts được sử dụng cho các trường hợp dễ lỗi và thường xuyên. Hiểu cách hoạt động của Asserts không chỉ giúp ứng dụng không chết 1 cách dễ dàng mà còn giúp những lập trình viên khác dễ đọc code hơn.

```objective-c

+ (NSArray *)indexPathsFromRow:(NSInteger)startRow toRow:(NSInteger)endRow inSection:(NSInteger)section {
NSAssert(endRow >= startRow, @"endRow must be greater or equal than startRow");

NSMutableArray *indexPaths = [NSMutableArray arrayWithCapacity:endRow-startRow+1];

for (NSInteger row = startRow; row <= endRow; row++) {
[indexPaths addObject:[NSIndexPath indexPathForRow:row inSection:section]];
}

return [indexPaths copy];
}

```

## Info.plist

The info.plist should always be named Info.plist and not Project_Info.plist or anything else.

## App Delegate

Đây là delegate của ứng dụng. Luôn đặt tên là AppDelegate không được thêm tiền tố như Project_AppDelegate hay gì khác.

## Other Sources to look at

* Apple HIG: http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html
* The Objective-C programming language: http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html
* http://stackoverflow.com/questions/155964/what-are-best-practices-that-you-use-when-writing-objective-c-and-cocoa
* http://cocoadevcentral.com/articles/000082.php
* http://cocoadevcentral.com/articles/000083.php

## Example Code

MSCanteenMenuParser.h

```objective-c

#import "MSJSONParser.h"

@class MSCanteen;    // class-directive instead of import

@interface MSCanteenMenuParser : MSJSONParser // no iVar-section

// the designated initializer
- (id)initWithJSON:(id)JSON canteen:(MSCanteen *)canteen;

@end
```

MSCanteenMenuParser.m

```objective-c

#import "MSCanteenMenuParser.h"
#import "MSCanteen.h" // import here to make known in implementation-file
#import "FKIncludes.h"

// Private Class Extension for private methods, properties etc.
@interface MSCanteenMenuParser ()

@property (nonatomic, strong) MSCanteen *canteen;

@end

@implementation MSCanteenMenuParser

@synthesize canteen = _canteen;    // synthesize with leading "_"

////////////////////////////////////////////////////////////////////////
#pragma mark - Lifecycle
////////////////////////////////////////////////////////////////////////

- (id)initWithJSON:(id)JSON canteen:(MSCanteen *)canteen {
if ((self = [super initWithJSON:JSON])) {
_canteen = canteen;
}

return self;
}

- (id)initWithJSON:(id)JSON {
// Usage of Assert
NSAssert(NO, @"Cannot parse menu without known canteen");
return [self initWithJSON:JSON canteen:nil];
}

////////////////////////////////////////////////////////////////////////
#pragma mark - MSJSONParser
////////////////////////////////////////////////////////////////////////

- (void)parseWithCompletion:(ms_network_block_t)completion {
// all menus of the given canteen
NSArray *menus = [self.JSON valueForKey:FKConstant.JSON.CanteenMenu.RootNode];

if (menus.count == 0) {
[self callCompletion:completion statusCode:MSStatusCodeJSONError];
}

// format of long method call
[FKPersistenceAction persistData:menus
entityName:[MSCanteenMenu entityName]
dictionaryIDKey:FKConstant.JSON.CanteenMenu.Name
databaseIDKey:[MSCanteenMenu uniqueKey]
updateBlock:^(MSCanteenMenu *canteenMenu, NSDictionary *data, NSManagedObjectContext *localContext) {
canteenMenu.canteen = [self.canteen inContext:localContext];

// parse the canteen menu itself
[canteenMenu setFromDictionary:data];
// parse the menu items
[canteenMenu setItemsFromArray:[data valueForKey:FKConstant.JSON.CanteenMenu.Items]];

[localContext save];
} completion:^{
[self callCompletion:completion statusCode:MSStatusCodeSuccess];
}];
}

@end


```

#### Attribution

Tài liệu này kế thừa bài viết của Sam Soffes. hãy chia sẻ nó cho mọi người. 
