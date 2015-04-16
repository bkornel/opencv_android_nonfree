My development environment is set up as follows:
================================================

- android-ndk-r10d (install path: `D:\adt-bundle-windows-x86_64-20140702\android-ndk-r10d\`)
- OpenCV-2.4.10-android-sdk (install path: `D:\CODE\OpenCV-2.4.10-android-sdk\`), [Download link][12]
- OpenCV-2.4.10 (install path: `D:\CODE\OpenCV-2.4.10\`), [Download link][13]

Building the nonfree module
---------------------------

 1. We actually only need to copy a few files from `OpenCV-2.4.10` source code to `OpenCV-2.4.10-android-sdk`, namely:<br />
 Copy the [nonfree][1] folder from `OpenCV-2.4.10\sources\modules\nonfree\include\opencv2\` to `OpenCV-2.4.10-android-sdk\sdk\native\jni\include\opencv2`.

 2. Create a folder to hold our new project for `libnonfree.so`. Here, I call it `libnonfree`. Create a `jni` folder under `libnonfree`. Copy the following files from `OpenCV-2.4.10\sources\modules\nonfree\src` to `libnonfree\jni\` folder:
 
 - [nonfree_init.cpp][2]
 - [precomp.hpp][3]
 - [sift.cpp][4] (*use the original file*)
 - [surf.cpp][5] (*use the original file*)
 
 3. Building `libnonfree.so`:<br />
 Create `Android.mk` and `Application.mk` scripts. This `Android.mk` is used to build `libnonfree.so`.
 
 - [Application.mk][6]
 - [Android.mk][7] (you should modify `OPENCV_PATH` where your `OpenCV-2.4.10-android-sdk` is)

 `cd` into the project folder `libnonfree` and type `ndk-build` to build the `libnonfree.so`.
  
So far, you have got `libnonfree.so` along with `libopencv_java.so` and `libgnustl_shared.so` in `libnonfree\libs\armeabi-v7a` folder.<br/>
You can easily build any SIFT or SURF applications using those libraries. If you want to use SIFT and SURF in JAVA code in your Android application, you only need to write JNI interfaces for the functions you want to use.

Building a sample application
-----------------------------

 1. Create a project folder call `libnonfree_demo`. Create a `jni` folder inside the project folder. Then copy `libnonfree.so` along with `libopencv_java.so` and `libgnustl_shared.so` into `jni`. 
 
 2. Create a [nonfree_jni.cpp][8] in `jni`. It is simple SIFT test program. It basically reads an image and detects the keypoints, then extracts feature descriptors, finally draws the keypoints to an output image.

 3. Create `Android.mk` and `Application.mk` inside `jni`:
 
 - [Application.mk][9]
 - [Android.mk][10] (you should modify `OPENCV_PATH` where your `OpenCV-2.4.10-android-sdk` is)

 `cd` into the project folder `libnonfree_demo` and type `ndk-build` to build the `libnonfree_demo.so`.

At this point you can easily extend the sample app with your `SVMDetector`. Just copy the source and include files int to the folder `libnonfree_demo\jni` and add cpp files to `LOCAL_SRC_FILES` in `Android.mk`.

The whole source can be downloaded from: [https://github.com/bkornel/opencv_android_nonfree][11].

Original source from: [http://web.guohuiwang.com/technical-notes/sift_surf_opencv_android][14]
 
  [1]: https://github.com/bkornel/opencv_android_nonfree/tree/master/_copy_to_opencv_sdk
  [2]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree/jni/nonfree_init.cpp
  [3]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree/jni/precomp.hpp
  [4]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree/jni/sift.cpp
  [5]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree/jni/surf.cpp
  [6]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree/jni/Application.mk
  [7]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree/jni/Android.mk
  [8]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree_demo/jni/nonfree_jni.cpp
  [9]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree_demo/jni/Application.mk
  [10]: https://github.com/bkornel/opencv_android_nonfree/blob/master/libnonfree_demo/jni/Android.mk
  [11]: https://github.com/bkornel/opencv_android_nonfree
  [12]: https://sourceforge.net/projects/opencvlibrary/files/opencv-android/2.4.10/OpenCV-2.4.10-android-sdk.zip/download
  [13]: https://sourceforge.net/projects/opencvlibrary/files/opencv-win/2.4.10/opencv-2.4.10.exe/download
  [14]: http://web.guohuiwang.com/technical-notes/sift_surf_opencv_android
  
  <script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-61966204-1', 'auto');
  ga('send', 'pageview');

</script>
