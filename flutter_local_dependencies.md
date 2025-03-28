# Setup ffmpeg local depedencies

This guide explains how to set up local FFmpeg dependencies for Android, iOS, and macOS projects.

# Table of Contents

1. [Setup FFmpeg Local Dependencies](#setup-ffmpeg-local-dependencies)
2. [Android Setup](#android-setup)
   1. [Create the libs Directory](#create-the-libs-directory)
   2. [Copy the FFmpeg Kit Library](#copy-the-ffmpeg-kit-library)
   3. [Update the build.gradle File](#update-the-build-gradle-file)
3. [iOS/macOS Setup](#iosmacos-setup)
   1. [For iOS](#for-ios)
      1. [Create the Frameworks Directory](#create-the-frameworks-directory-ios)
      2. [Extract and Copy the XCFramework](#extract-and-copy-the-xcframework)
      3. [Modify the Podspec File](#modify-the-podspec-file)
   2. [For macOS](#for-macos)
      - *(Follow the same steps as for iOS, applied to the macOS directory)*
4. [Usage](#usage)

## Android Setup

1. On `flutter/flutter/android` directory create a folder named `libs`;
Navigate to the flutter/flutter/android directory and create a folder named libs.

1. Copy the FFmpeg Kit Library
Copy the `ffmpeg-kit-**.aar library` from FFmpeg Kit into the newly created `libs` folder. Your folder structure should look similar to this:

```sh
 - android/
    - libs 
        - ffmpeg-kit-*
        - smart-exception-common-0.2.1.jar
        - smart-exception-java-0.2.1.jar
```

3. Update the `build.gradle` File
In your module’s `build.gradle` file, add the following configuration:

```gradle
....
rootProject.allprojects {
    repositories {
        ...
        flatDir {
            dirs project(':ffmpeg_kit_flutter').file('libs')
        }
    }
}

....
dependencies {
    ...
    api(name:'ffmpeg-kit-*',  ext:'aar')
    implementation files('libs/smart-exception-common-0.2.1.jar')
    implementation files('libs/smart-exception-java-0.2.1.jar')
}
```

## iOS/macOS Setup

The steps for macOS are the same as for iOS, but applied to their respective directories.

### For iOS

The same steps serves to `macos` version on the respective platform.

1. Create the `Frameworks` Directory
In the flutter/flutter/ios directory, create a folder named Frameworks.

2. Extract and Copy the XCFramework
Extract the `ffmpeg-kit-*-ios-xcframework.zip` archive, and copy the extracted folder (e.g., `ffmpeg-kit-min-gpl-*-ios-xcframework`) into the Frameworks directory:

```
 - ios/
    - Frameworks 
        - ffmpeg-kit-min-gpl-*-ios-xcframework
            - **zip-content**
```

3. Modify the Podspec File

Edit the `ffmpeg_kit_flutter_*.podspec` file with the following changes:

```podspec
  # comment or remove this section
  # s.default_subspec     = 'https'
  s.default_subspec = 'ffmpeg_kit_macos_local'
  
  s.dependency          'FlutterMacOS'
  s.pod_target_xcconfig = { 'DEFINES_MODULE' => 'YES' }

  s.subspec 'ffmpeg_kit_macos_local' do |ss|
    ss.vendored_frameworks = "Frameworks/ffmpeg-kit-*-macos-xcframework/ffmpegkit.xcframework", "Frameworks/ffmpeg-kit-*-macos-xcframework/libavcodec.xcframework", "Frameworks/ffmpeg-kit-*-macos-xcframework/libavdevice.xcframework", "Frameworks/ffmpeg-kit-*-macos-xcframework/libavfilter.xcframework", "Frameworks/ffmpeg-kit-*-macos-xcframework/libavformat.xcframework", "Frameworks/ffmpeg-kit-*-macos-xcframework/libavutil.xcframework", "Frameworks/ffmpeg-kit-*-macos-xcframework/libswresample.xcframework", "Frameworks/ffmpeg-kit-*-macos-xcframework/libswscale.xcframework"
  end

  # Comment or remove this section
  # s.subspec 'min' do |ss|
  #   ss.source_files         = 'Classes/**/*'
  #   ss.public_header_files  = 'Classes/**/*.h'
  #   ss.dependency 'ffmpeg-kit-macos-min', "6.0"
  #   ss.osx.deployment_target = '10.15'
  # end
```

## For MacOS

Follow the same steps as for iOS, applying them to the flutter/flutter/macos directory with the macos libs.

## Usage

To use the local dependencies in your project, update your pubspec.yaml file as follows:

```yaml
# Instead of:
ffmpeg_kit_flutter_*: 6.0.3

## Use: 
ffmpeg_kit_flutter:
    git: 
        url: <git-url>
        path: flutter/fluter
        ref: <branch-with-local-depedencies>
```

Replace <git-url> with your repository URL and <branch-with-local-dependencies> with the branch that contains the local dependencies setup.
