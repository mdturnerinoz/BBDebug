# BBDebug

This code has been downloaded from this [website](http://bigbucketsoftware.com/2008/05/18/objective-c-trace-statements/) to use as a debug/tracing meachanism (or at least a template for related ideas) for tracing/debugging code within a Cocoa (i.e. Mac) application.

## BBDebug Original Description

Lately, I’ve been trying to avoid calling NSLog directly in my Cocoa applications. I generally use NSLog to trace an application without having to resort to using breakpoints and such. The problem with calling NSLog directly is that it doesn’t automatically give any indication of the current location in the application. I have written two preprocessor definitions that take care of this:

### Header File

<pre>
#define BBTrace NSLog(@"%s", __PRETTY_FUNCTION__);

#define BBTraceInfo(s,...) \
{ \
    NSString* info = BBInfoString(s, ##__VA_ARGS__); \
    NSLog(@"%s (%@)", __PRETTY_FUNCTION__, info); \
    [info release]; \
}
</pre>
### Implementation
The implementation of BBInfoString is as follows:
<pre>
NSString* BBInfoString(NSString* format, ...)
{
    va_list args;
    va_start(args, format);
    NSString* info =
        [[NSString alloc] initWithFormat:format arguments:args];
    va_end(args);
    return info;
}
</pre>
###Basic Usage
<pre>
-(void)func
{
    BBTrace;
    if (isEnabled)
    {
        BBTraceInfo(@"enabled");
    }
}
</pre>
###Sample Output
<pre>
May 18 16:47:29 Machine App[72187]: -[SomeClass func]
May 18 16:47:29 Machine App[72187]: -[SomeClass func] (enabled)
</pre>

