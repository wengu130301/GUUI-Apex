# GUUI Apex 1.0

> 基于 **LineageOS 23.2（Android 16）** 的第三方 Android 定制系统，目标机型 **Xiaomi Mi MIX 2S（polaris / SDM845）**，以**蓝色虎皮鹦鹉**为核心设计语言。
>
> 由 [LaswI / 文文设计打造](https://github.com/wengu130301) 出品。

## 设计语言 · 蓝色虎皮鹦鹉
- **主色 鹦鹉蓝**（钴蓝梯度，核心值 `#2E66CF` / `#17409A`）作为系统全局主色调：状态栏、快捷开关、亮度条、锁屏、通知、强调色。
- 视觉资产（LaswI / 文文设计打造 版权所有）：
  - `laswi-logo` — 启动器/系统图标（白色圆角方块 + 蓝色线条字母 A，鸟翼/云朵造型，极简扁平）。
  - `apex.png` — "关于手机"页顶部 banner（像素「1」+ 鹦鹉头 + 虚化奇幻背景）。
  - `guguw.png` — 默认桌面/锁屏壁纸（蓝紫奇幻夜景：虎皮鹦鹉 + 发光蘑菇 + 满月 + 水晶，无文字水印，适配 1080×2160）。
  - `gugu's dream.mp3` → 转 OGG/Opus 后作为开机/来电/闹钟默认铃声。
- ⚠️ 资产 `apex.png` 字样为 **"GUVI APEX"**，按用户决定 **保持原样不改**（文档与代码统一用 **GUUI Apex**）。

## 工程方式（全云端）
> 注：壁纸/动画/铃声等二进制素材在本机只做必要的格式转换（sips / ffmpeg）与 zip 打包后上传 GitHub，**不涉及 AOSP 编译**。

- 所有源码改动 **只在 GitHub 完成**：fork 上游仓库 → 在 fork 上提交 GUUI Apex 改造 → 由 `guui-apex-manifest` 把 fork 串成完整构建。
- **本机不 clone AOSP、不编译**；实际出包需在具备 AOSP 构建环境的构建机（Linux + 大容量磁盘）上 `repo init` 本 manifest 后 `brunch polaris`。
- 版本一致性：设备树 / Settings / Launcher3 全部对齐到 **lineage-23.2** 分支；内核由 23.2 清单按上游 pin 引入（不单独覆盖）。

## 仓库索引
| 用途 | 上游 | GUUI Apex fork | 改造分支 |
| --- | --- | --- | --- |
| 设备树 polaris | LineageOS/android_device_xiaomi_polaris | wengu130301/android_device_xiaomi_polaris | guui-apex-1.0 |
| Settings（关于手机/版权页） | LineageOS/android_packages_apps_Settings | wengu130301/android_packages_apps_Settings | guui-apex-1.0 |
| Launcher3（桌面/壁纸/小组件） | LineageOS/android_packages_apps_Launcher3 | wengu130301/android_packages_apps_Launcher3 | guui-apex-1.0 |
| 构建清单 | — | wengu130301/guui-apex-manifest | main |
| 内核（调度微调，待做） | LineageOS/android_kernel_xiaomi_sdm845 | wengu130301/android_kernel_xiaomi_sdm845 | 未建（任务 42 时建） |

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
- [x] 1. 品牌替换 — 设备树 lineage_polaris.mk / system.prop 已改（产品名 guui_apex_polaris / 品牌 GuuiApex / 身份属性）
- [x] 43. device/xiaomi/polaris 设备属性增加 GUUI Apex 标识（ro.guui.apex.*）
- [x] 44. PRODUCT_NAME / PRODUCT_BRAND / PRODUCT_MODEL 指向 GUUI Apex（设备树 mk）
- [ ] build/make 仓库 PRODUCT 字段 — 需另 fork build/make（低优先级，设备树已覆盖）
### B. 开机/关机动画与壁纸
- [x] 2. 开机第一屏静态 logo → LaswI（bootanimation 首帧，1080×2160）
- [x] 3-5. bootanimation.zip 放入设备树并打包进 system/media、校验分辨率
- [x] 6. 关机动画配套 LaswI / 蓝色虎皮鹦鹉（shutdownanimation.zip，已开 config_animateShutdown）
- [x] 7. guguw.png → 默认桌面/锁屏壁纸（overlay default_wallpaper.png）
- [x] 10/13. 壁纸资源随系统打包、恢复出厂保持（overlay 自动生效）
- [ ] 8. Launcher3 默认壁纸指向 guguw（Settings/Launcher3 fork，待做）
- [ ] 9. SystemUI 锁屏壁纸指向 guguw（SystemUI fork，待做）
- [ ] 11. 默认壁纸预览图（WallpaperPicker，待做）
- [ ] 12. 首次开机向导默认壁纸（SetupWizard，待做）
- [ ] 28. apex.png 放入 overlay Settings 资源并声明（Settings fork，待做）
### C. 主题 / RRO / 图标
- [x] 14-17. 全局主色鹦鹉蓝 — colors_guui.xml（build overlay）+ GuuiApexThemeOverlay RRO（静态，Android 12+ 锁死）
- [ ] 15-16. 状态栏图标集 / Image 图标包 — **阻塞：缺少 Icon 图标文件夹资产**，需用户提供
- [ ] 18. 全局默认中文字体 — **阻塞：缺少字体二进制资产**，需用户提供
- [ ] 31. SystemUI 通知/锁屏时钟/充电动画融入鹦鹉元素（SystemUI fork，待做）
- [ ] 46. 修复中文排版 bug（待做）
### D. 铃声 / 音频（已完成）
- [x] 19-24. gugu's dream 转 OGG/Opus → 默认来电/通知/闹钟铃声 + 开机音（media/audio/* + config 默认值 + device.mk 打包）
### E. 设置 / 关于 / 控制中心
- [ ] 25-27. 关于手机页替换 GUUI Apex + apex.png 顶部大图 + 布局适配（Settings fork，待做）
- [ ] 29-30. LineageParts → GUUI Apex 控制中心 + 一键 GMS 开关（LineageParts fork，待做）
- [ ] 45. 新增 GUUI Apex 版权/开源声明页（Settings fork，待做）
### F. 桌面 Launcher3
- [ ] 32. 默认桌面布局 + 小组件 + 默认壁纸/图标包（Launcher3 fork，待做）
- [ ] 48. 预置蓝色虎皮鹦鹉动态小组件 — **阻塞：需开发/提供小组件应用**
### G. 隐私 / 去广告 / 去谷歌
- [x] 33-34. 关闭广告标识 API / 禁用 LineageOS metrics 统计上报（system.prop ro.lineage.metrics.enabled=false；LOS 本身无广告）
- [x] 36-37. 网络连通性校验改为关闭 captive 检测 + NTP 改 Aliyun（config overlay）
- [ ] 38. ROOT 一键开关 — **阻塞：需确定 root 方案（如 Magisk/KSU）集成**
- [ ] 39. 精简预置应用（device.mk，待做，需谨慎）
- [x] 47. 关闭推荐/广告弹窗（LOS 无 GMS 推荐，已天然满足）
- [ ] 41. OTA 服务器改为 GUUI Apex 自有（vendor/lineage 或 updater，待做）
### H. 本地化
- [ ] 35. 完整中文本地化，补全残留英文（overlay strings，待做）
### I. 内核 / 设备 / 预置应用
- [ ] 42. 内核调度微调适配 polaris（内核 fork，待做）
- [ ] 40. 拨号/联系人/短信 来电归属地 + 中文排序（Dialer/Contacts/Messaging fork，待做）
### J. Recovery
- [ ] 49. recovery 界面文字/背景替换 GUUI Apex / LaswI / 鹦鹉元素（recovery fork，待做）
### K. 收尾校验
- [x] 13/24. 恢复出厂后壁纸/铃声保持（overlay + media 自动生效）
- [ ] 50. 产品 mk 声明 overlay/图标包 + 固化国内 NTP/网络校验（NTP/网络已固化；图标包待资产）

## 说明
- 版本基线：**LineageOS 23.2 / Android 16**（polaris 当前最高可用且仍在维护，符合"越高越好"）。
- 一致性：所有 GUUI Apex fork 对齐 lineage-23.2；内核由 23.2 清单上游 pin 引入。
- 设计资产版权：LaswI / 文文设计打造。
- 阻塞项：Icon 图标包、中文字体、桌面动态小组件、ROOT 方案 —— 需用户补充资产/决策后方可闭环。
