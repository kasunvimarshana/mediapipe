## Installing MediaPipe

Note: To interoperate with OpenCV, OpenCV 3.x and above are preferred. OpenCV
2.x currently works but interoperability support may be deprecated in the
future.

Note: If you plan to use TensorFlow calculators and example apps, there is a
known issue with gcc and g++ version 6.3 and 7.3. Please use other versions.

Choose your operating system:

-   [Installing on Debian and Ubuntu](#installing-on-debian-and-ubuntu)
-   [Installing on CentOS](#installing-on-centos)
-   [Installing on macOS](#installing-on-macos)
-   [Installing on Windows Subsystem for Linux (WSL)](#installing-on-windows-subsystem-for-linux-wsl)
-   [Installing using Docker](#installing-using-docker)

To build and run Android apps:

-   [Setting up Android SDK and NDK](#setting-up-android-sdk-and-ndk)
-   [Setting up Android Studio with MediaPipe](#setting-up-android-studio-with-mediapipe)

To build and run iOS apps:

-   Please see the separate [iOS setup](./mediapipe_ios_setup.md) documentation.

### Installing on Debian and Ubuntu

1.  Checkout MediaPipe repository.

    ```bash
    $ git clone https://github.com/google/mediapipe.git

    # Change directory into MediaPipe root directory
    $ cd mediapipe
    ```

2.  Install Bazel (0.23 and above required).

    Option 1. Use package manager tool to install the latest version of Bazel.

    ```bash
    $ sudo apt-get install bazel

    # Run 'bazel version' to check version of bazel installed
    ```

    Option 2. Follow Bazel's
    [documentation](https://docs.bazel.build/versions/master/install-ubuntu.html)
    to install any version of Bazel manually.

3.  Install OpenCV.

    Option 1. Use package manager tool to install the pre-compiled OpenCV
    libraries.

    Note: Debian 9 and Ubuntu 16.04 provide OpenCV 2.4.9. You may want to take
    option 2 or 3 to install OpenCV 3 or above.

    ```bash
    $ sudo apt-get install libopencv-core-dev libopencv-highgui-dev \
                           libopencv-imgproc-dev libopencv-video-dev
    ```

    Option 2. Run [`setup_opencv.sh`] to automatically build OpenCV from source
    and modify MediaPipe's OpenCV config.

    Option 3. Follow OpenCV's
    [documentation](https://docs.opencv.org/3.4.6/d7/d9f/tutorial_linux_install.html)
    to manually build OpenCV from source code.

    Note: You may need to modify [`WORKSAPCE`] and [`opencv_linux.BUILD`] to
    point MediaPipe to your own OpenCV libraries, e.g., if OpenCV 4 is installed
    in "/usr/local/", you need to update the "linux_opencv" new_local_repository
    rule in [`WORKSAPCE`] and "opencv" cc_library rule in [`opencv_linux.BUILD`]
    like the following:

    ```bash
    new_local_repository(
        name = "linux_opencv",
        build_file = "@//third_party:opencv_linux.BUILD",
        path = "/usr/local",
    )

    cc_library(
      name = "opencv",
      srcs = glob(
          [
              "lib/libopencv_core.so*",
              "lib/libopencv_highgui.so*",
              "lib/libopencv_imgcodecs.so*",
              "lib/libopencv_imgproc.so*",
              "lib/libopencv_video.so*",
              "lib/libopencv_videoio.so*",

          ],
      ),
      hdrs = glob(["include/opencv4/**/*.h*"]),
      includes = ["include/opencv4/"],
      linkstatic = 1,
      visibility = ["//visibility:public"],
    )

    ```

4.  Run the [Hello World desktop example](./hello_world_desktop.md).

    ```bash
    $ export GLOG_logtostderr=1
    # Need bazel flag 'MEDIAPIPE_DISABLE_GPU=1' as desktop GPU is currently not supported
    $ bazel run --define MEDIAPIPE_DISABLE_GPU=1 \
        mediapipe/examples/desktop/hello_world:hello_world

    # Should print:
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    ```

### Installing on CentOS

1.  Checkout MediaPipe repository.

    ```bash
    $ git clone https://github.com/google/mediapipe.git

    # Change directory into MediaPipe root directory
    $ cd mediapipe
    ```

2.  Install Bazel (0.23 and above required).

    Follow Bazel's
    [documentation](https://docs.bazel.build/versions/master/install-redhat.html)
    to install Bazel manually.

3.  Install OpenCV.

    Option 1. Use package manager tool to install the pre-compiled version.

    Note: yum installs OpenCV 2.4.5, which may have an opencv/gstreamer
    [issue](https://github.com/opencv/opencv/issues/4592).

    ```bash
    $ sudo yum install opencv-devel
    ```

    Option 2. Build OpenCV from source code.

    Note: You may need to modify [`WORKSAPCE`] and [`opencv_linux.BUILD`] to
    point MediaPipe to your own OpenCV libraries, e.g., if OpenCV 4 is installed
    in "/usr/local/", you need to update the "linux_opencv" new_local_repository
    rule in [`WORKSAPCE`] and "opencv" cc_library rule in [`opencv_linux.BUILD`]
    like the following:

    ```bash
    new_local_repository(
        name = "linux_opencv",
        build_file = "@//third_party:opencv_linux.BUILD",
        path = "/usr/local",
    )

    cc_library(
      name = "opencv",
      srcs = glob(
          [
              "lib/libopencv_core.so*",
              "lib/libopencv_highgui.so*",
              "lib/libopencv_imgcodecs.so*",
              "lib/libopencv_imgproc.so*",
              "lib/libopencv_video.so*",
              "lib/libopencv_videoio.so*",

          ],
      ),
      hdrs = glob(["include/opencv4/**/*.h*"]),
      includes = ["include/opencv4/"],
      linkstatic = 1,
      visibility = ["//visibility:public"],
    )

    ```

4.  Run the [Hello World desktop example](./hello_world_desktop.md).

    ```bash
    $ export GLOG_logtostderr=1
    # Need bazel flag 'MEDIAPIPE_DISABLE_GPU=1' as desktop GPU is currently not supported
    $ bazel run --define MEDIAPIPE_DISABLE_GPU=1 \
        mediapipe/examples/desktop/hello_world:hello_world

    # Should print:
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    ```

### Installing on macOS

1.  Prework:

    *   Install [Homebrew](https://brew.sh).
    *   Install [Xcode](https://developer.apple.com/xcode/) and its Command Line
        Tools.

2.  Checkout MediaPipe repository.

    ```bash
    $ git clone https://github.com/google/mediapipe.git

    $ cd mediapipe
    ```

3.  Install Bazel (0.23 and above required).

    Option 1. Use package manager tool to install the latest version of Bazel.

    ```bash
    $ brew install bazel

    # Run 'bazel version' to check version of bazel installed
    ```

    Option 2. Follow Bazel's
    [documentation](https://docs.bazel.build/versions/master/install-ubuntu.html)
    to install any version of Bazel manually.

4.  Install OpenCV.

    Option 1. Use HomeBrew package manager tool to install the pre-compiled
    OpenCV libraries.

    ```bash
    $ brew install opencv
    ```

    Option 2. Use MacPorts package manager tool to install the OpenCV libraries.

    ```bash
    $ port install opencv
    ```

    Note: when using MacPorts, please edit the [`WORKSAPCE`] and
    [`opencv_linux.BUILD`] files like the following:

    ```bash
    new_local_repository(
      name = "macos_opencv",
      build_file = "@//third_party:opencv_macos.BUILD",
      path = "/opt",
    )

    cc_library(
      name = "opencv",
      srcs = glob(
        [
            "local/lib/libopencv_core.dylib",
            "local/lib/libopencv_highgui.dylib",
            "local/lib/libopencv_imgcodecs.dylib",
            "local/lib/libopencv_imgproc.dylib",
            "local/lib/libopencv_video.dylib",
            "local/lib/libopencv_videoio.dylib",
        ],
      ),
      hdrs = glob(["local/include/opencv2/**/*.h*"]),
      includes = ["local/include/"],
      linkstatic = 1,
      visibility = ["//visibility:public"],
    )
    ```

5.  Run the [Hello World desktop example](./hello_world_desktop.md).

    ```bash
    $ export GLOG_logtostderr=1
    # Need bazel flag 'MEDIAPIPE_DISABLE_GPU=1' as desktop GPU is currently not supported
    $ bazel run --define MEDIAPIPE_DISABLE_GPU=1 \
        mediapipe/examples/desktop/hello_world:hello_world

    # Should print:
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    ```

### Installing on Windows Subsystem for Linux (WSL)

1.  Follow the
    [instruction](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to
    install Windows Sysystem for Linux (Ubuntu).

2.  Install Windows ADB and start the ADB server in Windows.

    Note: Window’s and WSL’s adb versions must be the same version, e.g., if WSL
    has ADB 1.0.39, you need to download the corresponding Windows ADB from
    [here](https://dl.google.com/android/repository/platform-tools_r26.0.1-windows.zip).

3.  Launch WSL.

    Note: All the following steps will be executed in WSL. The Windows directory
    of the Linux Subsystem can be found in
    C:\Users\YourUsername\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_SomeID\LocalState\rootfs\home

4.  Install the needed packages.

    ```bash
    username@DESKTOP-TMVLBJ1:~$ sudo apt-get update && sudo apt-get install -y --no-install-recommends build-essential git python zip adb openjdk-8-jdk
    ```

5.  Install Bazel (0.23 and above required).

    ```bash
    username@DESKTOP-TMVLBJ1:~$ curl -sLO --retry 5 --retry-max-time 10 \
    https://storage.googleapis.com/bazel/0.27.0/release/bazel-0.27.0-installer-linux-x86_64.sh && \
    sudo mkdir -p /usr/local/bazel/0.27.0 && \
    chmod 755 bazel-0.27.0-installer-linux-x86_64.sh && \
    sudo ./bazel-0.27.0-installer-linux-x86_64.sh --prefix=/usr/local/bazel/0.27.0 && \
    source /usr/local/bazel/0.27.0/lib/bazel/bin/bazel-complete.bash

    username@DESKTOP-TMVLBJ1:~$ /usr/local/bazel/0.27.0/lib/bazel/bin/bazel version && \
    alias bazel='/usr/local/bazel/0.27.0/lib/bazel/bin/bazel'
    ```

6.  Checkout MediaPipe repository.

    ```bash
    username@DESKTOP-TMVLBJ1:~$ git clone https://github.com/google/mediapipe.git

    username@DESKTOP-TMVLBJ1:~$ cd mediapipe
    ```

7.  Install OpenCV.

    Option 1. Use package manager tool to install the pre-compiled OpenCV
    libraries.

    ```bash
    username@DESKTOP-TMVLBJ1:~/mediapipe$ sudo apt-get install libopencv-core-dev libopencv-highgui-dev \
                           libopencv-imgproc-dev libopencv-video-dev
    ```

    Option 2. Run [`setup_opencv.sh`] to automatically build OpenCV from source
    and modify MediaPipe's OpenCV config.

    Option 3. Follow OpenCV's
    [documentation](https://docs.opencv.org/3.4.6/d7/d9f/tutorial_linux_install.html)
    to manually build OpenCV from source code.

    Note: You may need to modify [`WORKSAPCE`] and [`opencv_linux.BUILD`] to
    point MediaPipe to your own OpenCV libraries, e.g., if OpenCV 4 is installed
    in "/usr/local/", you need to update the "linux_opencv" new_local_repository
    rule in [`WORKSAPCE`] and "opencv" cc_library rule in [`opencv_linux.BUILD`]
    like the following:

    ```bash
    new_local_repository(
        name = "linux_opencv",
        build_file = "@//third_party:opencv_linux.BUILD",
        path = "/usr/local",
    )

    cc_library(
      name = "opencv",
      srcs = glob(
          [
              "lib/libopencv_core.so*",
              "lib/libopencv_highgui.so*",
              "lib/libopencv_imgcodecs.so*",
              "lib/libopencv_imgproc.so*",
              "lib/libopencv_video.so*",
              "lib/libopencv_videoio.so*",

          ],
      ),
      hdrs = glob(["include/opencv4/**/*.h*"]),
      includes = ["include/opencv4/"],
      linkstatic = 1,
      visibility = ["//visibility:public"],
    )

    ```

8.  Run the [Hello World desktop example](./hello_world_desktop.md).

    ```bash
    username@DESKTOP-TMVLBJ1:~/mediapipe$ export GLOG_logtostderr=1

    # Need bazel flag 'MEDIAPIPE_DISABLE_GPU=1' as desktop GPU is currently not supported
    username@DESKTOP-TMVLBJ1:~/mediapipe$ bazel run --define MEDIAPIPE_DISABLE_GPU=1 \
        mediapipe/examples/desktop/hello_world:hello_world

    # Should print:
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    ```

### Installing using Docker

This will use a Docker image that will isolate mediapipe's installation from the rest of the system.

1.  [Install Docker](https://docs.docker.com/install/#supported-platforms) on
    your host sytem.

2.  Build a docker image with tag "mediapipe".

    ```bash
    $ git clone https://github.com/google/mediapipe.git
    $ cd mediapipe
    $ docker build --tag=mediapipe .

    # Should print:
    # Sending build context to Docker daemon  147.8MB
    # Step 1/9 : FROM ubuntu:latest
    # latest: Pulling from library/ubuntu
    # 6abc03819f3e: Pull complete
    # 05731e63f211: Pull complete
    # ........
    # See http://bazel.build/docs/getting-started.html to start a new project!
    # Removing intermediate container 82901b5e79fa
    # ---> f5d5f402071b
    # Step 9/9 : COPY . /mediapipe/
    # ---> a95c212089c5
    # Successfully built a95c212089c5
    # Successfully tagged mediapipe:latest
    ```

3.  Run the [Hello World desktop example](./hello_world_desktop.md).

    ```bash
    $ docker run -it --name mediapipe mediapipe:latest

    root@bca08b91ff63:/mediapipe# GLOG_logtostderr=1 bazel run --define MEDIAPIPE_DISABLE_GPU=1 mediapipe/examples/desktop/hello_world:hello_world

    # Should print:
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    # Hello World!
    ```

<!-- 4.  Uncomment the last line of the Dockerfile

    ```bash
    RUN bazel build -c opt --define MEDIAPIPE_DISABLE_GPU=1 mediapipe/examples/desktop/demo:object_detection_tensorflow_demo
    ```

    and rebuild the image and then run the docker image

    ```bash
    docker build --tag=mediapipe .
    docker run -i -t mediapipe:latest
    ``` -->

### Setting up Android SDK and NDK

Requirements:

*   Android SDK release 28.0.3 and above.
*   Android NDK r17c and above.

MediaPipe recommends setting up Android SDK and NDK via Android Studio, and see
[next section](#setting-up-android-studio-with-mediapipe) for Android Studio
setup. However, if you prefer using MediaPipe without Android Studio, please run
[`setup_android_sdk_and_ndk.sh`] to download and setup Android SDK and NDK
before building any Android example apps.

If Android SDK and NDK are already installed (e.g., by Android Studio), set
$ANDROID_HOME and $ANDROID_NDK_HOME to point to the installed SDK and NDK.

```bash
export ANDROID_HOME=<path to the Android SDK>
export ANDROID_NDK_HOME=<path to the Android NDK>
```

Please verify all the necessary packages are installed.

*   Android SDK Platform API Level 28 or 29
*   Android SDK Build-Tools 28 or 29
*   Android SDK Platform-Tools 28 or 29
*   Android SDK Tools 26.1.1
*   Android NDK 17c or above

### Setting up Android Studio with MediaPipe

The steps below use Android Studio to build and install a MediaPipe example app.

1.  Install and launch Android Studio.

2.  Select `Configure` | `SDK Manager` | `SDK Platforms`.

    *   Verify that Android SDK Platform API Level 28 or 29 is installed.
    *   Take note of the Android SDK Location, e.g.,
        `/usr/local/home/Android/Sdk`.

3.  Select `Configure` | `SDK Manager` | `SDK Tools`.

    *   Verify that Android SDK Build-Tools 28 or 29 is installed.
    *   Verify that Android SDK Platform-Tools 28 or 29 is installed.
    *   Verify that Android SDK Tools 26.1.1 is installed.
    *   Verify that Android NDK 17c or above is installed.
    *   Take note of the Android NDK Location, e.g.,
        `/usr/local/home/Android/Sdk/ndk-bundle`.

4.  Set environment variables `$ANDROID_HOME` and `$ANDROID_NDK_HOME` to point
    to the installed SDK and NDK.

    ```bash
    export ANDROID_HOME=/usr/local/home/Android/Sdk
    export ANDROID_NDK_HOME=/usr/local/home/Android/Sdk/ndk-bundle
    ```

5.  Select `Configure` | `Plugins` install `Bazel`.

6.  Select `Import Bazel Project`.

    *   Select `Workspace`: `/path/to/mediapipe`.
    *   Select `Generate from BUILD file`: `/path/to/mediapipe/BUILD`.
    *   Select `Finish`.

7.  Connect an Android device to the workstation.

8.  Select `Run...` | `Edit Configurations...`.

    *   Enter Target Expression:
        `//mediapipe/examples/android/src/java/com/google/mediapipe/apps/facedetectioncpu`
    *   Enter Bazel command: `mobile-install`
    *   Enter Bazel flags: `-c opt --config=android_arm64` select `Run`

[`WORKSAPCE`]: https://github.com/google/mediapipe/tree/master/WORKSPACE
[`opencv_linux.BUILD`]: https://github.com/google/mediapipe/tree/master/third_party/opencv_linux.BUILD
[`setup_opencv.sh`]: https://github.com/google/mediapipe/tree/master/setup_opencv.sh
[`setup_android_sdk_and_ndk.sh`]: https://github.com/google/mediapipe/tree/master/setup_android_sdk_and_ndk.sh
