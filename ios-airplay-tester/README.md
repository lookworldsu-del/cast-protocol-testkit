# iOS AirPlay 测试端规划

[返回项目首页](../README.md)

**状态：规划中。当前目录没有源码、安装包或实测结果。**

## 定位

为投影仪 AirPlay 接收方案提供可控的媒体与画面输入，记录公开 API 可见事件，并与外部测量及接收端证据关联。系统媒体路由选择与系统屏幕镜像分别设计测试流程。

## 第一版范围

| 功能 | 方案 | 验收证据 |
| --- | --- | --- |
| 标准媒体播放 | 基于 AVFoundation / AVKit；先验证少量自有或许可明确的素材 | 媒体清单、播放事件、接收端实际画面 |
| 媒体路由选择 | 使用 AVRoutePickerView，由测试人员选择目标接收端 | 操作记录、公开路由/外部播放状态、实际出画 |
| 播放控制 | 播放、暂停和能力范围内的 Seek；随后增加可重复的动作序列 | 动作时间线与接收端表现 |
| 应用事件 | 播放状态、外部播放状态、错误和可获得的播放器日志 | 带 run_id 的结构化导出；字段不可用时注明原因 |
| 镜像测试画面 | 帧号、计时、横竖屏与边界图形 | 控制中心镜像操作记录、同一相机拍摄的源屏与投影画面 |
| 结果导出 | 用例、媒体、版本、时间线及证据引用 | 可导入统一测试报告的记录 |

## 能力边界

- `AVRoutePickerView` 属于 AVKit，是媒体输出设备选择控件；不将其描述为任意指定设备、无人值守控制系统镜像的接口。
- 全屏镜像第一版由测试人员通过控制中心启动/停止，App 负责测试画面和记录。
- 公开 API 的播放日志不等同于 AirPlay 完整发现、鉴权、握手或接收端解码日志；后续按实验条件补充外部证据。
- 仅使用系统 AirPlay 与自行浏览 Bonjour 是不同路径。若后续增加直接局域网访问或服务浏览，再按实际 API 配置隐私说明、服务类型及所需授权，不把额外权限视为系统路由选择的统一前提。
- DRM / HDCP 验证需有合法可用的内容和相应能力条件。第一版不建设商业授权服务，不以保护内容黑屏直接认定接收端缺陷。
- 系统版本、媒体格式与接收端兼容范围以[设备能力矩阵](../docs/设备能力矩阵.md)的实际记录为准。

## 待确认事项

1. 可用 iPhone / iPad 型号、系统构建号与开发环境。
2. 投影仪型号、固件及 AirPlay 接收软件名称、版本和开启条件。
3. 可用非保护素材及接收端声明能力。
4. 人工操作、自动动作与外部测量各自可覆盖的阶段。

相关文档：[协议原理对比](../docs/协议原理对比.md)、[分协议用例](../docs/分协议测试用例清单.md)、[指标测量](../docs/指标测量方法.md)。

## 官方参考

- [AVRoutePickerView](https://developer.apple.com/documentation/avkit/avroutepickerview)
- [AVPlayer 外部播放状态](https://developer.apple.com/documentation/avfoundation/avplayer/isexternalplaybackactive)
- [iPhone AirPlay 与控制中心屏幕镜像](https://support.apple.com/guide/iphone/stream-video-and-audio-from-your-iphone-iphd668e80e6/ios)
- [TN3179：局域网隐私与系统高层服务的区别](https://developer.apple.com/documentation/technotes/tn3179-understanding-local-network-privacy)
