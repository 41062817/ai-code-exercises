## Memory Usage Optimization (Java)

Performance Issue Description:
-The application passes a bunch of high resolution images through a process, by loading all images into memory first. It then forms a new list of all the processed pictures and saves them.

-This results in the consumption of excessive memory by the application particularly during 50-100 images processing. The Java Virtual Machine (JVM) default heap size is 256MB causing the application to run out of heap space and crash with the following error message:

java.lang.OutOfMemoryError: Java heap space

You see the stack trace, it occurs in applyEffects() method with creating of a new BufferedImage object.

## Root Cause Analysis

The primary issue is that too much data is kept in memory simultaneously in the application.

The problem is a memory issue that is caused by the following parts:

-All of the original images are stored in the images list.
-All processed images are stored in the list called processedImages.
-The high-resolution images need a large amount of memory.
-Once all the images are processed, the application saves and releases memory.

This would allow hundreds of megabytes of image data to be stored simultaneously.

## Suggested Optimization

Instead, it's better to process images one by one.

The process should be improved to:

-Load one of the pictures in the folder.
-Use the grayscale effect.
-Don't let the image go out of your mind – save it right away.
-Free references to the picture to allow Java to release memory.
-Continue with the next image.

This eliminates the need for the images and processedImages lists and can significantly save memory.

A more long-term solution is to improve the code, but increasing the JVM heap size is another possible improvement using JVM options like -Xmx and -Xms.




## Performance Before and After Optimization

# Measurement	                                       Before Optimization	                                                  After Optimization
Memory usage	                                   Very high because all images are stored in memory	                 Much lower because only one image is stored at a time
Number of images processed	                       Crashes around 50–100 high-resolution images	                         Can process much larger batches successfully
Application stability	                           Often crashes with OutOfMemoryError	                                 Runs reliably without memory errors
Processing approach	                               Batch processing in memory	                                         Stream-like processing one image at a time


## Optimization Process

The optimization process was carried out in the following manner:

-Examined the error message and pinpointed the source of the memory issue.
-Studied code to analyse the method of loading and storing images.
-Recognized that it was not necessary to have both original and processed pictures in a list.
-Re-designed it in such a way that each picture is loaded, processed and saved as a separate image.
-Made it more memory efficient to run.

## Reflection Questions

# What impact did the optimization have on your memory management understanding?

It made me realize that having unnecessary objects in memory can be the cause of performance issues. It may be more efficient to process data in smaller chunks than loading it all at once.

# What improvements did you make in performance? Did they have an important impact that should warrant code changes?

Due to the optimization, the memory usage was significantly reduced and the issue of the application crashing was avoided. This improvement was considerable, as it now allows larger sets of images to be processed without needing to expand the computer's memory.

# What lessons have learned about performance bottlenecks that didn't know previously?

I found out that not all performance issues are due to slow code. Sometimes, the problem is with the way resources, such as memory, are used.

# How would you address related performance issues going forward?

I would first look at the error message, the resource usage of the parts of the program which use the most resources, look for unnecessary data in memory. I would then make some minor adjustments and see if it became better.

# How would you find the same problems in advance with the tools and/or techniques?

I would use tools like JVM memory monitoring tools/logging too/performance profilers to see how much memory the application consumes and whether there are any performance bottlenecks.