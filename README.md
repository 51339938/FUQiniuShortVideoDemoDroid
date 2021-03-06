﻿# FUQiniuShortVideoDemo（android）

## 概述

FUQiniuShortVideoDemo 是集成了 Faceunity 面部跟踪和虚拟道具功能和七牛短视频 SDK 的 Demo 。
本文是 FaceUnity SDK 快速对接七牛短视频 SDK 的导读说明，关于 FaceUnity SDK 的更多详细说明，请参看 [FULiveDemo](https://github.com/Faceunity/FULiveDemoDroid/tree/dev).

# 快速集成方法
## 添加module
添加faceunity module到工程中，在app dependencies里添加`compile project(':faceunity')`
## 修改代码
### 初始化与监听回调
在VideoRecordActivity的
onCreate方法中添加
```
mFURenderer = new FURenderer.Builder(this).build();

mShortVideoRecorder.setVideoFilterListener(new PLVideoFilterListener() {

    @Override
    public void onSurfaceCreated() {
        //初始化并加载美颜道具、默认道具
        mFURenderer.loadItems();
    }

    @Override
    public void onSurfaceChanged(int width, int height) {
    }

    @Override
    public void onSurfaceDestroy() {
        //销毁道具
        mFURenderer.destroyItems();
    }

    @Override
    public int onDrawFrame(int texId, int texWidth, int texHeight, long timeStampNs, float[] transformMatrix) {
        //渲染道具到原始数据上
        return mFURenderer.onDrawFrame(texId, texWidth, texHeight, transformMatrix);
    }
});

mShortVideoRecorder.setCameraPreviewListener(new PLCameraPreviewListener() {
    @Override
    public boolean onPreviewFrame(byte[] data, int width, int height, int rotation, int fmt, long tsInNanoTime) {
        //获取camera数据用于人脸追踪
        return mFURenderer.onPreviewFrame(data, width, height, rotation, fmt, tsInNanoTime);
    }
});
```
## 修改默认美颜参数
修改faceunity中faceunity中以下代码
```
private float mFaceBeautyALLBlurLevel = 1.0f;//精准磨皮
private float mFaceBeautyType = 0.0f;//美肤类型
private float mFaceBeautyBlurLevel = 0.7f;//磨皮
private float mFaceBeautyColorLevel = 0.5f;//美白
private float mFaceBeautyRedLevel = 0.5f;//红润
private float mBrightEyesLevel = 1000.7f;//亮眼
private float mBeautyTeethLevel = 1000.7f;//美牙

private float mFaceBeautyFaceShape = 4.0f;//脸型
private float mFaceBeautyEnlargeEye = 0.4f;//大眼
private float mFaceBeautyCheekThin = 0.4f;//瘦脸
private float mFaceBeautyEnlargeEye_old = 0.4f;//大眼
private float mFaceBeautyCheekThin_old = 0.4f;//瘦脸
private float mChinLevel = 0.3f;//下巴
private float mForeheadLevel = 0.3f;//额头
private float mThinNoseLevel = 0.5f;//瘦鼻
private float mMouthShape = 0.4f;//嘴形
```
参数含义与取值范围参考[这里](http://www.faceunity.com/technical/android-beauty.html)，如果使用界面，则需要同时修改界面中的初始值。

------
**七牛短视频 SDK：** https://developer.qiniu.com/pili/sdk/3734/android-short-video-sdk