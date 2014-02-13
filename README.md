AutoReportDealloc<br>
Automatically report object dealloc<br>
Attach in you main.m file
=================

<pre>
<code>
#import <objc/runtime.h>
const static char __DEALLOCKEY__;
@interface DeallocInfo:NSObject
@property (nonatomic, copy) NSString *msg;
@end

@implementation DeallocInfo

-(void)dealloc{
    NSLog(@"%@ dealloc",self.msg);
}
@end


@interface NSObject (LOGDealloc)
-(void)logOnDealloc;
@end


@implementation NSObject(LOGDealloc)
-(void)logOnDealloc{
    if (!objc_getAssociatedObject(self, &__DEALLOCKEY__)) {
        DeallocInfo *deallocInfo = [DeallocInfo new];
        [deallocInfo setMsg:NSStringFromClass(self.class)];
        objc_setAssociatedObject(self, &__DEALLOCKEY__, deallocInfo, OBJC_ASSOCIATION_RETAIN);
    }
}

-(id)newInit{
    id x = [self newInit];
    if (![self isKindOfClass:[DeallocInfo class]]) {
        [x logOnDealloc];
    }
    return x;
}
@end

</code>
</pre>
