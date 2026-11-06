# Android DLNA / Miracast 测试端规划

[返回项目首页](../README.md)

**状态：规划中。当前目录没有源码、安装包或实测结果。**

## 定位

DLNA 模块规划为可观测的 UPnP AV 控制端；Miracast 模块规划为受支持设备上的系统投屏测试辅助。两者共用用例编号、媒体/画面标识及结果格式，分别记录实现边界。

## DLNA：优先完成的协议闭环

1. SSDP 搜索、响应去重与失效处理，保存可获得的原始报文。
2. 读取设备和服务描述，识别 MediaRenderer 与服务版本，记录可用动作及媒体能力信息。
3. 使用接收端实际支持的 AVTransport / RenderingControl 等动作，分别验证设置 URI、播放、暂停、停止、Seek 与音量。
4. 对控制请求、响应、超时、错误码及状态查询分别记录；不以 SOAP 成功响应替代实际出画判定。
5. 使用 PC/Python 可控 HTTP 源先建立媒体链路。手机本地 HTTP 源作为独立场景，单独验证生命周期和后台限制。
6. 对媒体地址不可达、HTTP 错误、设备离线和动作超时设计可重复异常；协议异常参数实验在隔离测试环境中逐步开展。

实现或选用的 UPnP 库、支持服务/动作与设备差异在开发阶段确定。当前不宣称“Android SDK 原生自带完整 DLNA 实现”，也不宣称 DLNA 认证。

## Miracast：系统能力与测量辅助

| 计划能力 | 第一版做法 |
| --- | --- |
| 连接操作 | 核实手机与接收端确实支持 Miracast，记录厂商系统入口及人工操作；可用时提供入口跳转辅助 |
| 画面输入 | 提供帧号、计时器、旋转和画面边界测试图 |
| 结果记录 | 关联连接、断开、恢复操作与实际投影画面；不可观察的系统事件标为未知 |
| 性能测量 | 同一相机拍源屏与投影画面，按统一方法计算延迟及误差 |
| 诊断采集 | 设备开放 ADB 等诊断条件时按可用范围采集；没有权限则记录证据缺口 |

AOSP 的 Wi-Fi Display 扫描/连接控制涉及隐藏 API 和受限权限，不能作为普通 App 的通用公开能力。`WifiP2pManager` 提供发现和组网能力，不等于已经实现 Miracast 的协商、会话和媒体传输。第一版不自行实现完整协议栈，不依赖反射隐藏 API 达成通用兼容。

## 待确认事项

- 手机型号、Android 构建号、厂商投屏实现与实际采用的协议。
- 接收端型号、固件、接收软件及其能力声明；系统显示“Cast”不直接证明它使用 Miracast。
- 无线并发条件、P2P/基础设施路径，以及网络扰动实际作用的位置。
- 可获得的接收端诊断信息及人工步骤。

相关文档：[架构设计](../docs/架构设计.md)、[设备能力矩阵](../docs/设备能力矩阵.md)、[分协议用例](../docs/分协议测试用例清单.md)、[排查框架](../docs/投屏问题排查框架.md)。

## 官方参考

- [UPnP AV Architecture](https://upnp.org/specs/av/UPnP-av-AVArchitecture-v2.pdf)
- [UPnP AVTransport](https://upnp.org/specs/av/UPnP-av-AVTransport-v1-Service.pdf)
- [Android Wi-Fi Direct / P2P](https://developer.android.com/develop/connectivity/wifi/wifip2p)
- [AOSP DisplayManager：Wi-Fi Display API](https://android.googlesource.com/platform/frameworks/base/+/refs/heads/main/core/java/android/hardware/display/DisplayManager.java)
