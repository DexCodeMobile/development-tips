## The root cause

There is a known problem for the Swift community, about how the compiler generates some artifacts, used by LLDB to process the symbols during the debug session.

The generated DWARFs contain stored data in absolute paths; that means those data are bounded to fixed paths at the machine that generated it.

Once a developer tries to import those DWARFs, the absolute path, which usually comes from the $HOME folder of the generator user, doesn’t exist in his machine, and LLDB emits an error saying that it doesn’t find the specified path in DWARF.

As this is a compilation problem, there is no easy way to fix that.

And the situation gets worse, because the symbolization tool searches everywhere in your machine for those DSYMs, using SpotLight, an Apple tool that indexes all files, to provide a faster and assertive search.

The only solution here is to remove the troublemakers from your machine: the DSYMs.

![A8F02FDE-552F-40F6-B1A4-9AF17BA355FA](https://github.com/DexCodeMobile/development-tips/assets/1080918/63ca05ee-eb17-45b9-9153-05cbe378cff9)


## How to fix it

First, we need to identify which dsym is causing the problem.

For this, let’s get the images of UUIDs that have been loaded in app execution.

Run the app, then place a breakpoint anywhere in your code. When the execution stops, run the following command in lldb:

image list

The LLDB will return a list containing all the loaded images with their UUIDs, as shown in the next image:

#blindPeople: the image shows the image list command after its execution, followed by a list of all the loaded images in the app.

Get the UUID of the image that is causing the error.

<img width="1409" alt="Screen Shot 2021-11-17 at 4 06 41 PM" src="https://github.com/DexCodeMobile/development-tips/assets/1080918/721adc44-32ef-446e-853b-74fbc6049fa8">

Looking for the framework in the image list, we will find the following UUID:

<img width="1648" alt="Screen Shot 2021-11-17 at 4 08 57 PM" src="https://github.com/DexCodeMobile/development-tips/assets/1080918/6f0ed5bf-52a2-40c0-8e72-a34905c918b3">

#blindPeople: the image shows a marked UUID, followed by its description, indicating that this UUID belongs to the framework called XYPTO.

Next, let’s run the command that will search for the broken dsym and delete it, running:

`mdfind -0 "com_apple_xcode_dsym_uuids == UUID-DA-IMAGEM-AQUI" | xargs -0 rm -rf --`

In our example, we will run the command as:

`mdfind -0 "com_apple_xcode_dsym_uuids == 78A4025C-C667-3D4A-B415-DB4C078C048C" | xargs -0 rm -rf --`

Next, let’s erase the DerivedData folder. It’s important to erase all the data in that folder because we may find some left references for the dsym from previous compilations.

If your DerivedData folder is set as default, run the following code. Otherwise, look for the folder and erase it manually.

`rm -rf $HOME/Library/Developer/Xcode/DerivedData`

In the end, just build and run your application, and the LLDB’s commands like po will be available and working again.




