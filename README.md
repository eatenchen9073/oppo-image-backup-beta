# OPPO 影像备份 · 0.7.19 测试版

这是一个面向 Apple 芯片 Mac 的本地 OPPO 影像备份测试应用。它通过 USB 文件传输（MTP）读取手机媒体，把用户确认的副本保存到 Mac 上选择的目录。

> 当前仓库用于受邀测试。0.7.19 是未公证测试版，尚未完成干净 Mac 安装验收和多 OPPO 机型覆盖；请先阅读[安装与测试说明](docs/INSTALL-TEST.md)。

## 核心设计

- **首次备份完整素材，之后只备份新增内容。**第一次备份会列出 `DCIM` 和 `Pictures` 范围内可识别的照片、RAW 与视频。后续使用同一个备份库时，App 会根据本地备份记录找出新增或未完成项目，已存在的有效副本不会重复复制。
- **先检查，再由用户确认复制。**连接手机后可重新检测设备并刷新素材清单；重新检测本身不复制文件。开始备份前，用户可以确认素材范围和电脑上的保存目录。
- **按媒体类型分类。**照片、RAW 和视频分别保存到独立目录，视频不按分辨率拆分。
- **保留手机原件。**App 从手机读取文件，并在 Mac 目标目录写入副本；不会删除或修改手机原件。
- **本地处理。**当前版本不使用账号、云同步或云端备份。

## 当前版本

- 版本：0.7.19 Apple 芯片真机测试版
- 系统：macOS 15 或更新版本（包括 macOS 26、27）
- 架构：Apple Silicon（arm64）；不支持 Intel Mac
- 连接：手机需解锁，USB 用途设为“文件传输／MTP”，并允许文件访问
- 范围：只处理 MTP 可见、位于 `DCIM` 与 `Pictures` 中的受支持媒体；不备份应用数据或整机内容
- 签名：本机临时签名；没有 Developer ID 签名或 Apple 公证

每款 OPPO 的 MTP 兼容性尚未逐一验证。支持情况以实际测试结果为准。增量识别依赖来源路径和文件大小；同一路径、大小不变但内容已改变的文件可能无法被识别为变化。

## 下载和测试

从仓库的 [Releases](https://github.com/eatenchen9073/oppo-image-backup-beta/releases) 下载 0.7.19 测试包、SHA-256 校验文件和安装说明。仅使用独立的新测试目录；首次备份前先确认手机 `DCIM`、`Pictures` 里没有不愿复制到测试 Mac 的素材。首次备份可能包含范围内的全部可识别媒体。

未公证测试包可能触发 macOS 开发者验证提示。请先核对 SHA-256，并确认包来自你信任的发布者；如果系统提示 App 已损坏或包含恶意内容，请停止，不要绕过该提示。详细流程见[安装与测试说明](docs/INSTALL-TEST.md)。

测试反馈只需提供 Mac 芯片、macOS 版本、手机型号、MTP 状态、素材数量级和结果。请不要上传个人照片、RAW、视频、设备序列号或包含个人文件名的日志。

## 从源码构建

完整源码、构建脚本和所需依赖归档可从 [0.7.19 发布页下载](https://github.com/eatenchen9073/oppo-image-backup-beta/releases/tag/v0.7.19-beta)（文件名为 `OPPOBackup-Dev-0.7.19-source.zip`）。解压后，在 macOS 15+ 的 Apple 芯片 Mac 上安装 Xcode Command Line Tools，并在源码目录运行：

```sh
sh build-apple-demo.sh /absolute/path/to/new-output
```

构建脚本会创建一个新的输出目录。不要把手机资料或正式备份目录作为构建输出位置。模拟测试可在源码目录运行：

```sh
python3 -m unittest discover -s tests -v
```

第三方组件、数据来源和署名信息见源码包内的 `third_party/README.md`、`third_party/licenses/`、`assets/geonames/GEONAMES_NOTICE.txt` 和 `assets/ui/NOTICE.md`。

## 许可与再使用

应用自身尚未指定开源许可证。本仓库目前用于受邀测试；查看源码不代表获得复制、修改或再发布应用代码的授权。第三方组件和数据分别受其自身许可约束，详见对应 NOTICE 与许可证文件。
