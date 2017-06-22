---
title: EasyPr
date: 2017-06-22 16:21:51
tags: [Android,EasyPr]
categories: 笔记
---
<blockquote class="blockquote-center">EasyPr<br/>
---站在巨人的肩膀上前行
</blockquote>
<!-- more -->
> 需要一个车牌识别功能，对比几家车牌识别，收费都挺高的，权衡下还是选择了使用EasyPr。

## 查看github Demo

从[EasyPr](https://github.com/liuruoze/EasyPR)的github上面看到，已经有大佬整合到[Android](https://github.com/linuxxx/EasyPR_Android)项目的demo，下载下来，运行，看效果。

## 了解到的知识

#### 如何打包so
- 首先确认，你的**ndk-build.cmd**文件存在。  
- 然后**win+R**，打开命令行窗口。  
- 进入**jni**目录下。  
- 运行**ndk-build.cmd**文件，等一会包就打好了，打好的包会在平级目录的libs下。
![打包so](http://onifre9rp.bkt.clouddn.com/%E6%89%93%E5%8C%85so.png)

#### 打包so时的配置文件 Android.mk
[Android Api](https://developer.android.com/ndk/guides/android_mk.html?hl=zh-cn)参考官方api

 Android.mk 文件用于定义 Application.mk、构建系统和环境变量所未定义的项目范围设置。 它还可替换特定模块的项目范围设置。  
 
**LOCAL_PATH** 变量,表示源文件在开发树中的位置。在这里，构建系统提供的宏函数 my-dir 将返回当前目录（包含 Android.mk 文件本身的目录）的路径。
```
LOCAL_PATH := $(call my-dir)
```
**LOCAL_MODULE** 变量将存储您要构建的模块的名称。
```
LOCAL_MODULE := hello-jni
```
> 注：如果模块名称的开头已是 lib，则构建系统不会附加额外的前缀 lib；而是按原样采用模块名称，并添加 .so 扩展名。 因此，比如原来名为 libfoo.c 的源文件仍会生成名为 libfoo.so 的共享对象文件。 此行为是为了支持 Android 平台源文件从 Android.mk 文件生成的库；所有这些库的名称都以 lib 开头。

**TARGET_PLATFORM**作为构建系统目标的 Android API 级别号。
```
TARGET_PLATFORM := android-22
```
**TARGET_ABI**目标 Android API 级别与 ABI 的联接，特别适用于要针对实际设备测试特定目标系统映像的情况。 例如，要指定在 Android API 级别 22 上运行的 64 位 ARM 设备：
```
TARGET_ABI := android-22-arm64-v8a
```
**APP_ABI**指定不同的ABI

指令集|值
--- | ---
基于 ARMv7 的设备上的硬件 FPU 指令|	APP_ABI := armeabi-v7a
ARMv8 AArch64	|APP_ABI := arm64-v8a
IA-32|	APP_ABI := x86
Intel64	|APP_ABI := x86_64
MIPS32	|APP_ABI := mips
MIPS64 (r6)	|APP_ABI := mips64
所有支持的指令集	|APP_ABI := all

> 注：all 从 NDKr7 开始可用。


**APP_STL**变量指定要使用的运行时。 使用表中“名称”列中的值作为您的设置。 例如：
```
APP_STL := gnustl_static
```
只能为应用选择一个运行时，并且只能在 Application.mk 中选择。

名称	|说明	|功能
---|---|---
libstdc++（默认）|	默认最小系统 C++ 运行时库。|	不适用
gabi++_static|	GAbi++ 运行时（静态）。|	C++ 异常和 RTTI
gabi++_shared|	GAbi++ 运行时（共享）。|	C++ 异常和 RTTI
stlport_static|	STLport 运行时（静态）。|	C++ 异常和 RTTI；标准库
stlport_shared|	STLport 运行时（共享）。|	C++ 异常和 RTTI；标准库
gnustl_static|	GNU STL（静态）。|	C++ 异常和 RTTI；标准库
gnustl_shared|	GNU STL（共享）。|	C++ 异常和 RTTI；标准库
c++_static|	LLVM libc++ 运行时（静态）。|	C++ 异常和 RTTI；标准库
c++_shared|	LLVM libc++ 运行时（共享）。|	C++ 异常和 RTTI；标准库

## 遇到的问题
#### 第一个问题

刚下载好的demo，跑不起来，仔细看了一下,因为是用的Windows环境做开发，所以**ndk-build.cmd**要加**.cmd**的后缀。

解决了项目报错问题，然后发现，文件存储的路径是19版本的，为了兼容更低的版本，把文件存储路径顺手改掉。

运行，很愉快，没有在出现什么问题。

#### 第二个问题

demo是拍照识别的，但是老大希望做成扫描识别的，好吧。  
下载一个二维码识别demo，然后研究了一下二维码demo，然后把两个demo整合到一起了，把相机返回的图片，在进行二次截图，缩小识别范围（EasyPr的AndroidDemo就是这么做的），然后保存到文件中，然后识别，识别后的结果用正则效验，返回准确车牌，然后将结果展示到界面，并停止扫描。

#### 第三个问题

运行了整合扫描的项目，发现识别率变得特别低，排查发现，原二维码扫描，会保存，灰度图，导致EasyPr识别率变低。
```
public static Bitmap createBitmap (int[] colors, int width, int height, Bitmap.Config config) 
这个函数根据颜色数组来创建位图，注意:颜色数组的长度>=width*height

此函数创建位图的过程可以简单概括为为:更加width和height创建空位图，然后用指定的颜色数组colors来从左到右从上至下一次填充颜色。config是一个枚举，可以用它来指定位图“质量”。

public static Bitmap createBitmap (int[] colors, int offset, int stride, int width, int height, Bitmap.Config config)
此方法与2类似，但我还不明白offset和stride的作用
```
传入灰白两个颜色
```
public int[] renderThumbnail() {
		int width = getWidth() / THUMBNAIL_SCALE_FACTOR;
		int height = getHeight() / THUMBNAIL_SCALE_FACTOR;
		int[] pixels = new int[width * height];
		byte[] yuv = yuvData;
		int inputOffset = top * dataWidth + left;

		for (int y = 0; y < height; y++) {
			int outputOffset = y * width;
			for (int x = 0; x < width; x++) {
				int grey = yuv[inputOffset + x * THUMBNAIL_SCALE_FACTOR] & 0xff;
				pixels[outputOffset + x] = 0xFF000000 | (grey * 0x00010101);
			}
			inputOffset += dataWidth * THUMBNAIL_SCALE_FACTOR;
		}
		return pixels;
	}
```
于是改掉，完成。

#### 第四个问题
移植的时候要注意，包名保持一致，否者无法加载jni的代码
```
Java_com_aiseminar_EasyPR_PlateRecognizer_initPR
```

#### 第五个问题

Demo基本就完成了，接下来是往项目中添加了。  
这个时候遇到了这次最大的问题。  
我的测试设备有一台酷派的平板。悲剧了打包好的so包，在其他设备上都可以使用，唯独这台平板，怎么着都找不到so。  
算是学习的机会吧，又看了看so打包方法和一些相关的内容。然鹅，并没有用。。  
最后在同事的帮助下，发现是因为**EasyPr**的so中调用了**Opencv**的so包，导致**Opencv**的so包加载失败。
于是代码改成主动加载**opencv_java3**  



```
 static {
        try {
            System.loadLibrary("opencv_java3");
            System.loadLibrary("EasyPR");
        } catch (UnsatisfiedLinkError ule) {
            System.err.println("WARNING: Could not load library!");
        }
    }
```

> 吐槽一下，国内的各种系统真的是**深度定制**啊!