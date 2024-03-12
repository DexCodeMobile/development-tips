# xcode-debuggin
Xcode debugging tips

Important: Some may be available only on XCode 10

## Inserting code on the fly

What is it?
When the execution reaches a breakpoint, you can insert a whole set of new code, such as to change values of variables in scope and continue to see how the code behaves with this new code in place

Example: The following changes value of a variable in scope:
expression <var_name>=<new_value>
Note that the var_name should be in scope when it breaks

<img width="665" alt="Screen Shot 2018-08-22 at 9 07 55 AM" src="https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/73386306-fa4e-4f6b-b941-8abba78a0ce1">



## Conditional break point

What is it?
Breaks only when a condition evaluates to true.

Example: Set the condition to the following to make a break only when the variable has a particular value
<var_name> == <some_value>

<img width="849" alt="Screen Shot 2018-08-22 at 9 12 19 AM" src="https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/d05bf5c6-466f-4793-a3e8-1e8aafe9f17e">



## Log message on break points

What is it?
When you dont want to break, but just want to log a few things, such as value of a variable, this can be used

Example: To log value of a variable
My value is @<var_name>@
Note that, in order that we continue the execution after it logs, select "Automatically continue after evaluating actions"

<img width="664" alt="Screen Shot 2018-08-22 at 9 20 32 AM" src="https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/29f31db6-39fa-4c47-803c-82aa744e68b1">


## Symbolic breakpoint

What is it?
Breaks when a particular symbol, like a variable, is changed

Example: To break when setStringValue is called on a NSTextField, we add a symbolic break point to its parent class [NSControl setStringValue:]

Important: When you create a symbolic breakpoint, XCode resolves the breakpoint and if it resolves is rightly it adds a row beneath the breakpoint indicating the framework/library where this symbol is defined. If you don't see that new row, it means it has not resolved it rightly and the code would not break as expected. Therefore, as an example, if you set the breakpoint to [NSTextField setStringValue:] instead of NSControl, it would not resolve it and hence would not break. You should always create the break point to the actual implementation, and if that is in the parent class, you need to put that in there. You could right click on the symbol and know where that is actually implemented as shown.

<img width="901" alt="Screen Shot 2018-08-22 at 9 27 05 AM" src="https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/83045736-004a-49c2-ba22-7a0663da5172">


## Move instruction pointer

What is it?
Helps you to jump to different portion of code when it breaks. Great when you want to know what happens if a particular portion of the code is not executed.

Example: When the code breaks at a point, you can drag the break point using the square control to wherever you want as shown

Important: XCode gives you a warning that this may lead to unintended consequences since you may have moved the code to a place where many of the symbols are not initialized yet and we could crash. So, restrict this operation within the same function, and may be to the same block if possible

<img width="838" alt="Screen Shot 2018-08-22 at 9 36 35 AM" src="https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/d57f490b-2201-4501-998b-6d1a951c0073">



## Thread jumping

What is it?
Instead of manually dragging the instruction pointer to a different place, you could automatically perform this as well. So when it hits a break point, it simply jumps to a different place and starts execution. Again, take care to jump to a place where all symbols/variables are initialized there

Example: When you want to skip execution of a set of functions (which may be depending on other subsystems that are not available at that point of time) use the following:
thread jump --by <number of execution lines to skip>
<img width="721" alt="Screen Shot 2018-08-24 at 9 33 49 AM" src="https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/2a17bb82-1793-487a-9e50-e8de8ab6c676">



## Check parameters passed to a function

What is it?

When you want to check parameters passed to a function in a framework that you dont have the source code for, you can create a symbolic breakpoint for that function and use "po" to get the values passed on to it

Example: When it breaks, you use the transient registers that xcode maintains; $arg# to get the values
arg1 - Name of the object/class
arg2 - Name of the selector/function
arg3... argn - argument list

![Screen Shot 2018-08-30 at 7 19 18 PM](https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/0d398ee0-bc6b-4c2d-a752-b749efeb9f92)



## One-time breakpoint

What is it?

A breakpoint that happens just one time. For example, when you have already narrowed down to the area where a particular problem happens, such as an expected change happening to the text of a NSTextField after your function to update the text field returns, you can create a one-time break point then to break only once now whenever the text field gets updated the next time. These are mostly in multi threaded applications where multiple threads can update a variable and things can turn out unexpectedly

Example: To create such a break point for this [NSTextField setStringValue]
breakpoint set --one-shot true --name "[NSControl setStringValue:]"

![Screen Shot 2018-08-23 at 6 40 24 PM](https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/46b37358-67aa-477f-8544-a341fe9cefdb)


