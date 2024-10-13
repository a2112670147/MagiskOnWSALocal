这个自述文件是关于如何在Windows Subsystem for Android (WSA) 上安装Magisk和Google Apps（GApps）的指南。Magisk是一个在Android上提供root权限和系统修改功能的框架，而GApps是Google官方应用的集合，通常用于非Google品牌的Android设备。以下是该自述文件的翻译和一些必要的调整以确保其正常运行：

---

# Magisk on WSA（含Google Apps）

:warning: 对于分叉开发者：请不要使用GitHub Actions构建，因为GitHub会将你的分叉GitHub Actions使用量计入这个上游仓库，这可能导致这个上游仓库因为大量分叉构建GitHub Actions而被GitHub工作人员禁用，就像[MagiskOnWSA](https://github.com/LSPosed/MagiskOnWSA)一样。

## 支持以下系统的生成

- Linux (x86_64或arm64)

  需要以下依赖：

  | 操作系统 | 依赖项 |
  | --- | --- |
  | Debian | `lzip patchelf e2fsprogs python3 aria2 attr unzip sudo` |
  | openSUSE Tumbleweed | 同上 |
  | Arch | 同Debian |

  Python版本需要≥ **3.7.2**。

  - 推荐使用

    - Ubuntu (你可以使用[WSL2](https://apps.microsoft.com/store/search?publisher=Canonical%20Group%20Limited))

      开箱即用。

    - Debian (你可以使用[WSL2](https://apps.microsoft.com/store/detail/debian/9MSVKQC78PK6))

      开箱即用。

    - openSUSE Tumbleweed (你可以使用[WSL2](https://apps.microsoft.com/store/detail/opensuse-tumbleweed/9MSSK2ZXXN11))

      开箱即用。

    `run.sh`脚本会自动处理所有依赖项。

    无需输入任何命令。

## 特性

- 几分钟内通过几次点击即可集成Magisk和GApps
- 保持每个构建都是最新的
- 支持ARM64和x64
- 支持MindTheGapps
- 移除Amazon Appstore
- 修复VPN对话框不显示的问题（使用我们的[VpnDialogs应用](https://github.com/LSPosed/VpnDialogs)）
- 添加设备管理功能
- 无人值守安装
- 自动激活Windows 11的开发者模式
- 一键脚本更新到新版本同时保留数据
- 合并所有语言包

## 文本指南

1. 如果你喜欢，可以给项目加星。
2. 将仓库克隆到本地：

   ```bash
   git clone https://github.com/LSPosed/MagiskOnWSALocal.git  --depth 1
   ```

3. 运行 `cd MagiskOnWSALocal`。
4. 运行 `./scripts/run.sh`。
5. 选择WSA版本及其架构（通常是x64）。
6. 选择Magisk的版本。
7. 选择你想安装的GApps品牌：
   - MindTheGapps

    我们只能选择这一个。
8. 选择root解决方案（无表示无root）。
9. 如果你第一次运行脚本，它将需要一些时间来完成。脚本完成后，将在`MagiskOnWSALocal`文件夹中生成两个新文件夹`output`和`download`。进入`output`文件夹。在步骤3中运行`./run.sh`脚本时，如果你选择了`Do you want to compress the output?`，则在`output`文件夹中你将看到一个名为`WSA-with-magisk-stable-MindTheGapps_2207.40000.8.0_x64_Release-Nightly`的压缩文件，否则将是一个同名的文件夹。如果有一个文件夹，请打开它并跳到步骤10。注意：在`output`文件夹中生成的压缩文件或文件夹的名称可能对你来说不同。它将取决于执行`./run.sh`时所做的选择。
10. 解压压缩文件并打开解压后创建的文件夹。
11. 在这里找到`Run.bat`文件并运行它。
    - 如果你之前有MagiskOnWSA安装，它将在**保留所有用户数据**的同时自动卸载旧版本并安装新版本，所以不用担心你的数据。
    - 如果你有官方WSA安装，你应该先卸载它。（如果你想保留数据，可以在卸载前备份`%LOCALAPPDATA%\Packages\MicrosoftCorporationII.WindowsSubsystemForAndroid_8wekyb3d8bbwe\LocalCache\userdata.vhdx`，在安装后恢复它。）
    - 如果弹出窗口在**没有请求管理权限**的情况下消失，并且WSA没有成功安装，你应该手动以管理员身份运行`Install.ps1`：
        1. 按`Win+x`并选择`Windows Terminal (Admin)`。
        2. 输入`cd "{X:\path\to\your\extracted\folder}"`并按`enter`，记得替换`{X:\path\to\your\extracted\folder}`，包括花括号，例如`cd "D:\wsa"`
        3. 输入`PowerShell.exe -ExecutionPolicy Bypass -File .\Install.ps1`并按`Enter`。
        4. 脚本将运行并安装WSA。
        5. 如果这个变通方法不起作用，你的PC可能不支持WSA。
12. Magisk/Play Store将启动。享受安装LSPosed-Zygisk（启用Zygisk）或Riru和LSPosed-Riru的乐趣。

---

## 常见问题解答

<details open>

- 我可以删除已安装的文件夹吗？

  不可以。

- 如何更新WSA到较新版本？

  1. 更新构建脚本：

      ```bash
      git pull
      ```

      更多关于git的使用，请参考<https://git-scm.com/book> 

  2. 重新运行脚本，替换你之前安装的内容并重新运行`Install.ps1`。别担心，你的数据将被保留。

- 如何从WSA获取logcat？

  `%LOCALAPPDATA%\Packages\MicrosoftCorporationII.WindowsSubsystemForAndroid_8wekyb3d8bbwe\LocalState\diagnostics\logcat`

- 如何更新Magisk到较新版本？

  和更新WSA的步骤一样。

- 如何通过Play Integrity（以前称为SafetyNet）？

  和其他所有模拟器一样，没有办法。

- 虚拟化没有启用？

  `Install.ps1`会帮助你启用它，如果没有启用。重启后，重新运行`Install.ps1`来安装WSA。如果仍然不起作用，你可能需要在BIOS中启用虚拟化。这是一个很长的故事，所以请向谷歌寻求帮助。

- 如何重新挂载系统为读写？

  在WSA中没有办法，因为它被Hyper-V挂载为只读。你可以通过制作Magisk模块来修改系统。或者直接修改system.img。向谷歌寻求帮助。

- 我不能`adb connect localhost:58526`，该怎么办？

  确保开发者模式已启用。如果问题仍然存在，请检查WSA设置页面上的IP地址并尝试`adb connect ip:5555`。

- 为什么Magisk在线模块为空？

  Magisk积极移除了在线模块仓库。你可以本地安装模块，或者通过`adb push module.zip /data/local/tmp`和`adb shell su -c magisk --install-module /data/local/tmp/module.zip`来安装。

- 我可以使用Magisk v23.0稳定版或更低版本吗？

  不可以。Magisk有bug阻止其在WSA上运行。Magisk v24+已经修复了这些问题。所以你必须使用Magisk v24或更高版本。

- 如何摆脱Magisk？

  选择`none`作为root解决方案。

- 如何安装自定义GApps？

  [教程](Custom-GApps.md)

- 我可以从哪里下载MindTheGapps？

  你可以从这里下载[MindTheGapps](https://androidfilehost.com/?w=files&flid=322935) ([镜像](http://downloads.codefi.re/jdcteam/javelinanddart/gapps))。

  注意，没有x86_64预构建版本，所以你需要自己构建（[仓库](https://gitlab.com/MindTheGapps/vendor_gapps)）。

  或者你可以从[这个页面](https://sourceforge.net/projects/wsa-mtg/files/x86_64/)下载为12.1和13构建的包。

- 我可以切换OpenGApps到MindTheGapps并保留之前构建的用户数据吗？

  不可以。更改GApps品牌后，你应该擦除数据。否则，你会发现安装的GApps不被识别。

- 集成OpenGApps的WSA无法启动。

  OpenGApps尚未发布为Android 12L和13构建的版本，只发布了为Android 11构建的版本，可能不兼容从而导致崩溃。考虑切换到MindTheGapps。

- 如何安装KernelSU？

  [教程](KernelSU.md)

</details>

---

## 致谢

- [StoreLib](https://github.com/StoreDev/StoreLib)：下载WSA的API
- [Magisk](https://github.com/topjohnwu/Magisk)：Android上最著名的
