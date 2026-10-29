# GUUI Apex 1.0

> 基于 **LineageOS 22.2（Android 15）** 的第三方 Android 定制系统，目标机型 **Xiaomi Mi MIX 2S（polaris / SDM845）**，以**蓝色虎皮鹦鹉**为核心设计语言。
>
> 由 [LaswI / 文文设计打造](https://github.com/wengu130301) 出品。

## 设计语言 · 蓝色虎皮鹦鹉
- **主色 鹦鹉蓝**：取自 `laswi-logo` 的宝蓝/钴蓝线条色，作为系统全局主色调、状态栏、快捷开关、亮度条、锁屏元素。
- 视觉资产：
  - `laswi-logo` — 启动器/系统图标（白色圆角方块 + 蓝色线条字母 A，鸟翼/云朵造型，极简扁平）。
  - `apex.png` — "关于手机"页顶部 banner（像素「1」+ GUUI APEX 字样 + 蓝紫鹦鹉头，虚化奇幻背景）。
  - `guguw.png` — 默认桌面/锁屏壁纸（蓝紫奇幻夜景：虎皮鹦鹉 + 发光蘑菇 + 满月 + 水晶，无文字水印，适配 1080×2160）。
  - `gugu's dream.mp3` → 转 ogg 后作为开机/来电/闹钟默认铃声。
- ⚠️ 资产 `apex.png` 字样为 "GUVI APEX"，文档与代码统一使用 **GUUI Apex**（如需改图请重新出图）。

## 工程方式（全云端）
> 注：壁纸/动画等二进制素材在本机只做必要的格式转换与 zip 打包后上传 GitHub，不涉及 AOSP 编译。

- 所有源码改动 **只在 GitHub 完成**：fork 上游仓库 → 在 fork 上提交 GUUI Apex 改造 → 由 `guui-apex-manifest` 把 fork 串成完整构建。
- **本机不 clone AOSP、不编译**；实际出包需在具备 AOSP 构建环境的构建机（Linux + 大容量磁盘）上 `repo init` 本 manifest 后 `brunch polaris`。
- 已完成/进行中的 fork 见下方「仓库索引」。

## 仓库索引
| 用途 | 上游 | GUUI Apex fork |
| --- | --- | --- |
| 设备树 polaris | LineageOS/android_device_xiaomi_polaris | wengu130301/android_device_xiaomi_polaris |
| 构建清单 | — | wengu130301/guui-apex-manifest |

## 构建方式（构建机侧）
```bash
repo init -u https://github.com/wengu130301/guui-apex-manifest -b main
repo sync
. build/envsetup.sh
breakfast polaris
brunch polaris
```

## 任务进度（共 50 项，按文档 Code_20261008.txt）
### A. 品牌与构建标识
- [ ] 1. LineageOS 全部品牌文本替换为 GUUI Apex（build/设备树/输出名/版本标识）
- [ ] 44. build/make 调整 PRODUCT_NAME / PRODUCT_BRAND / PRODUCT_MODEL
- [ ] 43. device/xiaomi/polaris 设备属性增加 GUUI Apex 标识
### B. 开机/关机动画与壁纸（进行中：默认壁纸 + 开机/关机动画已完成；Launcher3/锁屏/向导/预览/apex 待续）
- [x] 2. 开机第一屏静态 logo → LaswI（适配 Mix2s 分辨率）
- [x] 3-5. bootanimation.zip 放入设备树并打包、校验 1080×2160
- [x] 6. 关机动画配套 LaswI / 蓝色虎皮鹦鹉
- [ ] 7-13. guguw.png 替换桌面/锁屏壁纸、Launcher3/SystemUI 配置、预览图、首次向导、恢复出厂保持
- [ ] 28. apex.png 放入 overlay Settings 资源并声明
### C. 主题 / RRO / 图标
- [ ] 14-17. 全局 RRO 主色鹦鹉蓝、状态栏图标集、Image 图标包默认启用
- [ ] 18. 全局默认中文字体
- [ ] 31. SystemUI 通知/锁屏时钟/充电动画融入鹦鹉元素
- [ ] 46. 修复中文排版 bug
### D. 铃声 / 音频
- [ ] 19-24. gugu's dream 转 ogg 并设为开机/来电/闹钟默认、打包 media、恢复出厂保持
### E. 设置 / 关于 / 控制中心
- [ ] 25-27. 关于手机页替换 GUUI Apex + apex.png 顶部大图 + 布局适配
- [ ] 29-30. LineageParts → GUUI Apex 控制中心 + 一键 GMS 开关
- [ ] 45. 新增 GUUI Apex 版权/开源声明页
### F. 桌面 Launcher3
- [ ] 32. 默认桌面布局 + GUUI Apex 小组件 + 默认壁纸/图标包
- [ ] 48. 预置蓝色虎皮鹦鹉动态小组件
### G. 隐私 / 去广告 / 去谷歌
- [ ] 33-34. 移除广告模块/统计上报
- [ ] 36-37. 网络连通性校验 / NTP 改为国内服务器
- [ ] 38. 预置 ROOT 一键开关
- [ ] 39. 精简预置应用
- [ ] 47. 关闭所有推荐/广告弹窗
- [ ] 41. OTA 服务器改为 GUUI Apex 自有
### H. 本地化
- [ ] 35. 完整中文本地化，补全残留英文
### I. 内核 / 设备 / 预置应用
- [ ] 42. 内核调度微调适配 polaris
- [ ] 40. 拨号/联系人/短信 来电归属地 + 中文排序
### J. Recovery
- [ ] 49. recovery 界面文字/背景替换 GUUI Apex / LaswI / 鹦鹉元素
### K. 收尾校验
- [ ] 13 / 24. 恢复出厂后壁纸/铃声保持
- [ ] 50. 产品 mk 声明 overlay/图标包 + 固化国内 NTP/网络校验

## 说明
- 版本基线：LineageOS 23.2 / Android 16（polaris 当前最高且仍在 nightly 维护，符合"越高越好"）。
- 设计资产版权：LaswI / 文文设计打造。