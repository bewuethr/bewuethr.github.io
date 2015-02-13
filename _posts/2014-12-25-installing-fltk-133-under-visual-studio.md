---
layout: post
title: Installing FLTK 1.3.3 under Visual Studio Community 2013
published: true
---


## Downloading and building FLTK

Download FLTK from [here](http://www.fltk.org/software.php): get **fltk-1.3.3-source.tar.gz** and decompress it, using for example [7-Zip](http://www.7-zip.org). You should end up with a directory **fltk-1.3.3**; I have put mine in a subfolder of my **Documents** folder:

![2014_12_25-01.png](/images/2014_12_25-01.png)

Now open the project file **fltk.sln** in **fltk-1.3.3/ide/VisualC2010**. If asked, say you're okay with updating the project files.

Make sure you have selected **Debug** as your solution configuration.

![2014_12_25-04.png](/images/2014_12_25-02.png)

In the context menu of the **demo** project, select **Set as StartUp Project**.

![2014_12_25-05.png](/images/2014_12_25-03.png)

Now build the solution (**Build** – **Build Solution** or Ctrl+Shift+B). If successful, you should be able to run the **demo** project (**Debug** – **Start Debugging** or F5).

Change the solution configuration to **Release** and build again. Check if you can run the **demo** project (F5).

Now we have to copy the generated library files to the right directory so Visual Studio finds them. In my installation, the libraries and headers are in **C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC** (just **VC** from now on). If you have the right directory, there should be a bunch of subdirectories:

![2014_12_25-06.png](/images/2014_12_25-04.png)

Now copy (don't move) the following from you **fltk-1.3.3** folder:

* The complete **FL** directory to **VC\include**
* All **.lib** files from **fltk-1.3.3\lib** (except **README.lib**) to **VC\lib**. There should be 14 files: seven pairs of two files each, one of which has an added "d" (for "debug") at the end of the file name.
* **fluid.exe** and **fluidd.exe** from **fltk-1.3.3\fluid** to **VC\bin**

## Setting up a FLTK project

To set up a FLTK project, select **File** – **New** – **Project** and choose **Win32 Project**:

![2014_12_25-07.png](/images/2014_12_25-05.png)

In the wizard under **Application Settings**, check **Empty project**:

![2014_12_25-08.png](/images/2014_12_25-06.png)

Create a source file with **Project** – **Add New Item**:

![2014_12_25-09.png](/images/2014_12_25-07.png)

Enter this code:

```cpp
#include<FL/Fl.H>
#include<FL/Fl_Box.H>
#include<FL/Fl_Window.H>

int main()
{
	Fl_Window window(200,200,"Window title");
    FL_Box box(0,0,200,200,"Hey, I mean, Hello, World!");
    window.show();
    return Fl::run();
}
```

Now open **Project** – **Properties** and make sure you have selected **Debug** in **Configuration** in the top left corner. Under **Linker** – **Input**, edit **Additional Dependencies** and add **fltkd.lib**, **wsock32.lib**, **comctl32.lib**, **fltkjpegd.lib** and **fltkimagesd.lib**, each on their own line:

![2014_12_25-10.png](/images/2014_12_25-08.png)

Because this is the **Debug** configuration, we would like to have a visible console window. To get that, go to **Linker** – **System** and set **/SUBSYSTEM:CONSOLE** for **SubSystem**:

![2014_12_25-11.png](/images/2014_12_25-09.png)

Now build and run the project. You should get a console window and the GUI window:

![2014_12_25-12.png](/images/2014_12_25-10.png)

Now go back to **Project** – **Properties** and select **Release** as the configuration. Go to **Linker** – **Input** and edit the **Additional Dependencies** just like before, but this time, add **fltk.lib**, **wsock32.lib**, **comctl32.lib**, **fltkjpeg.lib** and **fltkimages.lib** – notice the missing "d" at the end of the names:

![2014_12_25-13.png](/images/2014_12_25-11.png)

Make sure that under **Linker** – **System**, the option **/SUBSYSTEM:WINDOWS** is selected so we don't get a console window. Now build and run to see if only the GUI window shows up.

That's how you set up any project using FLTK!

![2014_12_25-14.png](/images/2014_12_25-12.png)
![2014_12_25-14.png](/images/2014_12_25-13.png)
![2014_12_25-14.png](/images/2014_12_25-14.png)
![2014_12_25-15.png](/images/2014_12_25-15.png)
![2014_12_25-16.png](/images/2014_12_25-16.png)
![2014_12_25-17.png](/images/2014_12_25-17.png)
![2014_12_25-18.png](/images/2014_12_25-18.png)
