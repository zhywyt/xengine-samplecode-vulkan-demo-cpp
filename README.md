# XEngine超分和自适应可变速率着色的使用(VULKAN)

## 简介

本篇Codelab主要介绍如何使用XEngine空域GPU超分API进行纹理的超分以及开启自适应可变速率着色，通过XComponent组件创建Vulkan环境，实现绘制SPONZA场景。本篇CodeLab使用Native C++模板创建。

## 效果预览

如图所示，XComponent组件绘制SPONZA场景，点击超分下拉选择菜单，切换超分模式，点击勾选框选择是否开启自适应可变速率着色

![XEngine Spatial Upscale](screenshots/device/XEngine_VULKAN_Spatial_Upscale_Example.jpg)

## 相关概念
- [XComponent](https://developer.harmonyos.com/cn/docs/documentation/doc-references-V3/_o_h___native_x_component-0000001497210885-V3?catalogVersion=V3)：可用于媒体数据写入，并显示在XComponent组件。

## 使用说明

1. 运行示例代码。
2. 点击下拉选择菜单，在no upscale（不使用超分）、spatial upscale（空域GPU超分）、fsr upscale（FSR1.0超分）三种模式间进行切换。
3. 点击勾选框，可以开启/关闭自适应可变速率着色。

## 工程目录
```
├── entry/src/main	         // 代码区
│  ├── cpp
│  │  ├── types
│  │  │  ├── libnativerender
             └── index.d.ts      // native层接口注册文件
│  │  │── napi_init.cpp          // native api层接口的具体实现函数
│  │  │── CMakeLists.txt         // native层编译配置
│  │  │── 3rdParty               // 三方件
│  │  │── commom                 // 通用接口
│  │  │── file                   // 文件管理
│  │  │── libs                   // 三方动态库
│  │  │── manager                // native&arkts交互
│  │  │── render                 // 渲染
│  │  │── vulkanbase             // vulkan基础能力封装
│  ├── ets
│  │  ├── entryability
             └── EntryAbility.ts // 程序入口类
│  │  ├── pages
             └── index.ets       // 主界面展示类
│  ├── resources                 // 资源文件目录
│  │  ├── base
│  │  │  ├── media
             └── icon.png        // 图片资源
│  │  ├── rawfile
│  │  │  ├── Sponza
             └── sponza.obj     // 模型资源
```

## 具体实现
本示例展示的功能使用了XEngine中定义的以下API：
* VKAPI_ATTR VkResult VKAPI_CALL HMS_XEG_EnumerateDeviceExtensionProperties(VkPhysicalDevice physicalDevice, uint32_t* pPropertyCount, XEG_ExtensionProperties* pProperties);
* VKAPI_ATTR VkResult VKAPI_CALL HMS_XEG_CreateSpatialUpscale(VkDevice device, const XEG_SpatialUpscaleCreateInfo*  pXegSpatialUpscaleCreateInfo, XEG_SpatialUpscale* pXegSpatialUpscale);
* VKAPI_ATTR void VKAPI_CALL HMS_XEG_CmdRenderSpatialUpscale(VkCommandBuffer commandBuffer, XEG_SpatialUpscale xegSpatialUpscale, XEG_SpatialUpscaleDescription* pXegSpatialUpscaleDescription);
* VKAPI_ATTR void VKAPI_CALL HMS_XEG_DestroySpatialUpscale(XEG_SpatialUpscale xegSpatialUpscale);
* VKAPI_ATTR VkResult VKAPI_CALL HMS_XEG_CreateAdaptiveVRS(VkDevice device, XEG_AdaptiveVRSCreateInfo* pXegAdaptiveVRSCreateInfo, XEG_AdaptiveVRS* pXegAdaptiveVRS);
* VKAPI_ATTR void VKAPI_CALL HMS_XEG_CmdDispatchAdaptiveVRS(VkCommandBuffer cmdBuffer, XEG_AdaptiveVRS xegAdaptiveVRS, XEG_AdaptiveVRSDescription* pXegAdaptiveVRSDescription);
* VKAPI_ATTR void VKAPI_CALL HMS_XEG_DestroyAdaptiveVRS(XEG_AdaptiveVRS xegAdaptiveVRS);
通过调用展示的API实现超分功能以及自适应可变速率着色功能。

## 相关权限

不涉及

## 依赖

* 本示例依赖assimp三方件，请按系统版本编译对应版本三方件。
* 3D模型资源："[Crytek Sponza](https://casual-effects.com/data/)" by Frank Meinl, Crytek is licensed under [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/)/replace "\\\" with "/" in file sponza.mtl

## 约束与限制

1. 本示例仅支持标准系统上运行，支持设备：支持Mate 60系列及以上的华为手机。
2. 本示例为Stage模型，支持API version 12。
3. HarmonyOS系统：HarmonyOS NEXT Developer Beta1及以上。
4. DevEco Studio版本：DevEco Studio NEXT Developer Beta1及以上。
5. HarmonyOS SDK版本：HarmonyOS NEXT Developer Beta1 SDK及以上。
