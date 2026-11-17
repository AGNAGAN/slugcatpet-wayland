# SlugcatPet Wayland

[![Linux](https://github.com/AGNAGAN/slugcatpet-wayland/actions/workflows/linux.yml/badge.svg)](https://github.com/AGNAGAN/slugcatpet-wayland/actions/workflows/linux.yml)
[![Windows](https://github.com/AGNAGAN/slugcatpet-wayland/actions/workflows/windows.yml/badge.svg)](https://github.com/AGNAGAN/slugcatpet-wayland/actions/workflows/windows.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

让 Rain World 的蛞蝓猫住进你的桌面。本仓库是
[lingxiaojun/slugcatpet](https://github.com/lingxiaojun/slugcatpet) 的 fork，
重点维护 **KDE Plasma / KWin Wayland** 原生支持，同时保留上游的 Windows 功能和原有玩法。

A Rain World slugcat desktop pet, with a native **KDE Plasma / KWin Wayland** port.
This is a fork of [lingxiaojun/slugcatpet](https://github.com/lingxiaojun/slugcatpet);
Windows support and the original gameplay are kept.

[中文](#中文) | [English](#english)

---

## 中文

### 简介

SlugcatPet 用 Python + PySide6 复刻《Rain World》蛞蝓猫的质点物理、行为状态机和皮肤系统。
猫会在屏幕上走动、爬杆、游泳、躲雨、打猎、分享食物、挨着睡，也会被光标拎起来或抢走光标。

本 fork 在上游基础上增加了 Native Wayland/KWin 适配、Linux 桌面集成、自定义全局热键、
按键喵、catless 模式、Lizard Pet，以及 Rain World 配乐选择性导入和自然排序。

### 功能

- **多猫同屏**：不同角色、性格、好感、饥饿/困倦/寒冷/业力和隐藏/死亡状态。
- **物理与行为**：两节质点身体、爬杆/横杆/杆网寻路、游泳/憋气/无重力、捕猎、分享食物、挨着睡、让路。
- **环境**：雨循环、挡雨窗口、洪水、暴风雪、无重力；物品和珍珠文字编辑。
- **外观**：Dress My Slugcat 皮肤，头/脸/身体/手臂/臀/腿/尾巴可混搭，支持颜色、配件和尾巴形状。
- **声音**：场景配乐、雨声、喵叫；可从正版 Rain World 选择性导入配乐。
- **交互**：托盘、全局热键、定格/单步、手动控制、状态 HUD、开机自启。
- **Linux/Wayland**：单 overlay + 动态输入区域、KWin 置顶/全屏检测/挡雨窗口/全局热键、光标写入 best-effort。
- **附加应用**：`lizardpet`（无猫模式）可从源码启动。

### 运行要求

| 平台 | 要求 |
| --- | --- |
| Windows 10 / 11 | Python 3.12+；已安装 Rain World 和 Downpour DLC |
| Linux | KDE Plasma 6 / KWin 6（已在 Plasma 6.8 测试）、Wayland 会话、Python 3.12+、[uv](https://docs.astral.sh/uv/)；已安装 Rain World 和 Downpour DLC |
| Linux 光标写入 | Qt 6.12+，且合成器支持 `wp_pointer_warp_v1` |

GNOME、Sway 等非 KWin 合成器可以运行，但置顶、全屏检测、挡雨窗口和全局热键会降级。

首次启动需要选择 Rain World 根目录（其中应包含 `RainWorld_Data`）。程序会把所需的图像和音乐
提取到 `~/.slugcatpet/assets`，不会修改游戏文件。

### 安装与运行

#### Windows

从源码运行：

```powershell
git clone https://github.com/AGNAGAN/slugcatpet-wayland.git
cd slugcatpet-wayland
uv sync
uv run run_slugcatpet.py
```

也可以直接用 pip：

```powershell
pip install PySide6 UnityPy numpy Pillow
python run_slugcatpet.py
```

本 fork 目前以源码运行为主，Releases 暂未附带 Windows exe。需要现成 exe 时可以使用上游的
[v1.3.4 release](https://github.com/lingxiaojun/slugcatpet/releases/tag/v1.3.4)，但其中不包含本 fork
的 Wayland 和本地功能改动。

#### Linux（KDE Plasma / Wayland）

```bash
git clone https://github.com/AGNAGAN/slugcatpet-wayland.git
cd slugcatpet-wayland
./packaging/linux/install.sh
```

安装脚本会用 `uv tool` 创建隔离环境，并安装应用菜单入口和图标。安装后从应用菜单启动
**SlugcatPet**，或运行：

```bash
slugcatpet
```

也可以直接从源码运行：

```bash
uv sync
uv run run_slugcatpet.py
```

卸载：

```bash
./packaging/linux/uninstall.sh
```

卸载不会删除 `~/.slugcatpet` 中的设置、素材、皮肤和日志。

### Wayland / KWin 集成

当前会话会自动加载运行时 KWin 脚本。若希望登录后仍然生效，请在
**设置 → Wayland / KWin** 勾选 **登录后自动加载 KWin 脚本**。

勾选后程序会把脚本安装到 `~/.local/share/kwin/scripts/slugcatpet/`，写入 `kwinrc`，并调用
`org.kde.KWin.reconfigure`。该开关默认关闭，程序不会在未启用时修改 `kwinrc`。

Wayland 光标抢取是 **best-effort**，不是系统级指针锁定：左/右键单击会解除，指针持续移动到
另一个输出时也会结束抢取并留在新输出。详细说明见
[`ai-docs/PHASE5_CURSOR_WRITE.md`](ai-docs/PHASE5_CURSOR_WRITE.md)，
安装/卸载和故障排查见 [`packaging/linux/README.md`](packaging/linux/README.md)。

### 操作

默认全局快捷键如下，可在 **设置 → 键位** 中修改：

| 功能 | 默认快捷键 |
| --- | --- |
| 喵叫（按住决定长短） | `Alt+M` |
| 定格 / 解除定格 | `Ctrl+Alt+P` |
| 显示 / 隐藏状态面板 | `Ctrl+Alt+H` |
| 查看雨循环 | `Ctrl+Alt+R` |
| 中止光标劫持 | `Ctrl+Alt+Q` |
| 退出程序 | `Ctrl+Alt+X` |
| 定格时前进一帧 | `Ctrl+Alt+Right` |
| 定格时前进 10 帧 | `Ctrl+Alt+Shift+Right` |

全局快捷键需要带 `Ctrl`、`Alt` 或 `Win`。如果按下没有反应，通常是被其他程序占用，
可以在键位设置里换一个组合。

### 音乐

在音乐窗口的「导入」菜单选择「从 Rain World 导入配乐…」，或在
**设置 → 声音 → Rain World 配乐** 打开同一个窗口。程序会读取当前游戏目录中的配乐，
支持搜索、勾选、全选/全不选，并把选中的曲目复制到：

```text
~/.slugcatpet/music/Rain World
```

不会修改游戏文件。少数资源包曲目会解码成 wav，体积比 ogg 大。音乐列表按目录和文件名
自然排序（`1, 2, …, 10, 11, …, 100`）。

### 皮肤

支持 Dress My Slugcat 皮肤。创意工坊订阅的皮肤会自动出现；其他皮肤放进皮肤文件夹
（**设置 → 打开皮肤文件夹**）即可。

在设置里点击猫旁边的「外观」，可以给头、脸、身体、手臂、臀、腿、尾巴分别选择皮肤和颜色，
也可以调整尾巴形状、开关配件。皮肤格式和画法见
[Dress My Slugcat](https://steamcommunity.com/sharedfiles/filedetails/?id=2948971756)。

### 数据目录

所有用户数据都在 `~/.slugcatpet/`：

```text
app.json          设置和存档
assets/           从游戏提取的图像、音乐和音效
music/            导入的配乐
skins/            Dress My Slugcat 皮肤
hotkeys.json      自定义快捷键
slugcatpet.log    运行日志
kwin/             运行时 KWin 脚本
```

删除该目录会重置所有设置，并需要重新导入游戏素材。

### 开发

```bash
uv sync
uv run python -m unittest discover -s tests
uv run ruff check .
uv run ruff format --check .
uv run python tools/params_doc.py --check
```

行为回归需要本机 Rain World 素材：

```bash
uv run python tests/behavior_check.py check
```

行为检查的物理状态可以跨平台比较；渲染帧哈希受 Qt、平台插件和字体抗锯齿影响，
不同平台之间不保证一致。

项目结构和扩展文档：

- [`ai-docs/ARCHITECTURE.md`](ai-docs/ARCHITECTURE.md)：整体架构和 tick 流程
- [`ai-docs/CHARACTERS.md`](ai-docs/CHARACTERS.md)：新增角色
- [`ai-docs/PARAMS.md`](ai-docs/PARAMS.md)：行为参数
- [`verification/README.md`](verification/README.md)：Wayland/KWin 验收记录

文档以中文为主。

### 附加应用：Lizard Pet

仓库还包含 `lizardpet`（catless 模式），可从源码启动：

```bash
uv run run_lizardpet.py
```

目前它没有单独的 Linux 安装入口。

### 已知限制

- Wayland 光标抢取是 best-effort，不是系统级锁定；左/右键或移动到其他输出会解除。
- 非 KWin 合成器会降级：没有置顶、全屏检测、挡雨窗口和全局热键。
- 本 fork 暂未在 Releases 提供 Windows exe，Windows 请从源码运行，或使用上游发布包。
- 首次启动必须能读取 Rain World 和 Downpour 素材；仓库不包含游戏素材。

### 素材与版权

- 本仓库不包含任何 Rain World 游戏素材。全部游戏图像和音乐都在你本机、从你自己的正版安装中提取。
- 皮肤归各自的作者所有。本程序只在你本机读取皮肤，不会打包或分发。
- 此项目为粉丝项目。Rain World 及相关名称、美术均归其权利人所有。
- 本仓库代码以 MIT 许可发布（见 [`LICENSE`](LICENSE)）；该许可仅覆盖本仓库代码，不授予任何游戏素材相关权利。
- `slugcatpet/audio/fsbogg.py` 部分改写自 [python-fsb5](https://github.com/HearthSim/python-fsb5)
  （MIT，Simon Pinfold）；`vorbis_setups.py` 中的数据由 libvorbis（BSD，Xiph.Org）生成。

### 上游与版本

- 上游项目：[lingxiaojun/slugcatpet](https://github.com/lingxiaojun/slugcatpet)
- 本 fork 当前基于上游 **v1.3.4**（`0853bdb`），合并提交 `9053500`
- 上游演示视频：[Bilibili 合集](https://space.bilibili.com/1810584454/lists?sid=8504199)
- 本地改动以 Native Wayland/KWin 适配和 Linux 桌面集成为主；上游版本更新会继续合并。

如果只使用 Windows，且不需要本 fork 的 Wayland/本地功能，上游发布包通常更省事。
Wayland 相关问题请反馈到本仓库，上游玩法问题请反馈到上游仓库。

---

## English

### What it is

SlugcatPet is a Rain World slugcat desktop pet built with Python and PySide6. It recreates the
game's mass-point physics, behavior state machine, skins, rain cycle, items, and social interactions.

This fork keeps the upstream Windows app and adds a native **KDE Plasma / KWin Wayland** port,
custom global hotkeys, push-to-meow, catless mode, Lizard Pet, Rain World music import, and natural
sorting.

### Features

- **Multiple cats**: different characters, personalities, bonds, hunger, sleep, cold, karma, and hidden/dead states.
- **Physics and behavior**: two-chunk bodies, poles and beam networks, swimming, diving, zero gravity, hunting, food sharing, huddling, and yielding.
- **Environment**: rain cycles, rain-blocking windows, floods, blizzards, zero gravity, items, and pearl text editing.
- **Appearance**: Dress My Slugcat skins with per-part mixing, colors, accessories, and tail shape.
- **Audio**: scene music, rain, meows, and selective Rain World music import.
- **Interaction**: tray, global hotkeys, freeze/step, manual control, status HUD, and autostart.
- **Linux/Wayland**: one overlay with dynamic input regions, KWin topmost/fullscreen/rain-shelter/global-hotkey support, and best-effort cursor writing.
- **Extra app**: `lizardpet` (catless mode) can be run from source.

### Requirements

| Platform | Requirements |
| --- | --- |
| Windows 10 / 11 | Python 3.12+; Rain World and the Downpour DLC installed |
| Linux | KDE Plasma 6 / KWin 6 (tested on Plasma 6.8), a Wayland session, Python 3.12+, and [uv](https://docs.astral.sh/uv/); Rain World and the Downpour DLC installed |
| Linux cursor writing | Qt 6.12+ and compositor support for `wp_pointer_warp_v1` |

GNOME and Sway can run the app, but topmost, fullscreen detection, rain shelters, and global hotkeys
are degraded without the KWin bridge.

On first launch, choose your Rain World root folder (the one containing `RainWorld_Data`). Images and
music are extracted to `~/.slugcatpet/assets`; the game files are not modified.

### Install and run

#### Windows

From source:

```powershell
git clone https://github.com/AGNAGAN/slugcatpet-wayland.git
cd slugcatpet-wayland
uv sync
uv run run_slugcatpet.py
```

Or with pip:

```powershell
pip install PySide6 UnityPy numpy Pillow
python run_slugcatpet.py
```

This fork currently runs from source and does not ship a Windows exe in its Releases. If you want a
ready-made exe, the upstream [v1.3.4 release](https://github.com/lingxiaojun/slugcatpet/releases/tag/v1.3.4)
is available, but it does not include this fork's Wayland or local changes.

#### Linux (KDE Plasma / Wayland)

```bash
git clone https://github.com/AGNAGAN/slugcatpet-wayland.git
cd slugcatpet-wayland
./packaging/linux/install.sh
```

The installer creates an isolated `uv tool` environment and installs the application menu entry and
icon. Launch **SlugcatPet** from the menu, or run:

```bash
slugcatpet
```

To run from source instead:

```bash
uv sync
uv run run_slugcatpet.py
```

Uninstall:

```bash
./packaging/linux/uninstall.sh
```

The uninstaller does not delete settings, assets, skins, or logs in `~/.slugcatpet`.

### Wayland / KWin integration

The runtime KWin script is loaded for the current session automatically. To keep it after login,
enable **Settings → Wayland / KWin → Load KWin script at login**.

This installs the script to `~/.local/share/kwin/scripts/slugcatpet/`, updates `kwinrc`, and calls
`org.kde.KWin.reconfigure`. The toggle is off by default; the app does not modify `kwinrc` until you
enable it.

Cursor grabbing on Wayland is **best-effort**, not a system-level pointer lock. Left/right click
releases it; sustained movement to another output also ends the grab and leaves the pointer there.
See [`ai-docs/PHASE5_CURSOR_WRITE.md`](ai-docs/PHASE5_CURSOR_WRITE.md) for details and
[`packaging/linux/README.md`](packaging/linux/README.md) for install/uninstall and troubleshooting.

### Controls

Default global shortcuts (change them in **Settings → Keys**):

| Action | Default shortcut |
| --- | --- |
| Meow (hold for long meow) | `Alt+M` |
| Freeze / unfreeze | `Ctrl+Alt+P` |
| Show/hide status panel | `Ctrl+Alt+H` |
| Show rain cycle | `Ctrl+Alt+R` |
| Abort cursor hijack | `Ctrl+Alt+Q` |
| Quit | `Ctrl+Alt+X` |
| Step one frame while frozen | `Ctrl+Alt+Right` |
| Step 10 frames while frozen | `Ctrl+Alt+Shift+Right` |

Global shortcuts need `Ctrl`, `Alt`, or `Win`. If a shortcut does nothing, another application
probably owns it; choose a different combination in the key settings.

### Music

Open the Music window's **Import** menu and choose **Import Rain World music…**, or use
**Settings → Sound → Rain World music**. The app scans the current game folder, supports search and
selection, and copies the selected tracks to:

```text
~/.slugcatpet/music/Rain World
```

Game files are not modified. A few packaged tracks are decoded to wav and are larger than ogg.
The music list is sorted naturally by folder and filename (`1, 2, …, 10, 11, …, 100`).

### Skins

Dress My Slugcat skins are supported. Workshop subscriptions appear automatically; put other skins
in the skins folder (**Settings → Open skins folder**).

In Settings, click **Appearance** next to a cat to choose skins and colors for the head, face, body,
arms, hips, legs, and tail, adjust the tail shape, and toggle accessories. See
[Dress My Slugcat](https://steamcommunity.com/sharedfiles/filedetails/?id=2948971756) for the skin
format.

### Data directory

All user data lives in `~/.slugcatpet/`:

```text
app.json          settings and save data
assets/           images, music, and sounds extracted from the game
music/            imported music
skins/            Dress My Slugcat skins
hotkeys.json      custom shortcuts
slugcatpet.log    runtime log
kwin/             runtime KWin scripts
```

Deleting this directory resets all settings and requires importing game assets again.

### Development

```bash
uv sync
uv run python -m unittest discover -s tests
uv run ruff check .
uv run ruff format --check .
uv run python tools/params_doc.py --check
```

The behavior regression needs local Rain World assets:

```bash
uv run python tests/behavior_check.py check
```

Physics results are comparable across platforms; rendered-frame hashes depend on Qt, platform plugins,
and font antialiasing, so they are not expected to match across platforms.

Documentation:

- [`ai-docs/ARCHITECTURE.md`](ai-docs/ARCHITECTURE.md): architecture and tick flow
- [`ai-docs/CHARACTERS.md`](ai-docs/CHARACTERS.md): adding characters
- [`ai-docs/PARAMS.md`](ai-docs/PARAMS.md): behavior parameters
- [`verification/README.md`](verification/README.md): Wayland/KWin verification notes

The documentation is mostly in Chinese.

### Extra app: Lizard Pet

The repository also contains `lizardpet` (catless mode). Run it from source with:

```bash
uv run run_lizardpet.py
```

It currently has no separate Linux installer entry.

### Known limitations

- Wayland cursor grabbing is best-effort, not a system-level lock; left/right click or moving to another output releases it.
- Non-KWin compositors run in a degraded mode without topmost, fullscreen detection, rain shelters, or global hotkeys.
- This fork does not currently provide a Windows exe in Releases; run from source or use the upstream release.
- The first launch requires a local Rain World and Downpour installation; no game assets are bundled.

### Assets and copyright

- This repository contains no Rain World assets. All game images and music are extracted on your machine from your own copy.
- Skins belong to their authors. The app only reads them on your machine and never bundles or redistributes them.
- This is a fan project. Rain World and all related names and artwork belong to their respective owners.
- The code is released under the MIT license (see [`LICENSE`](LICENSE)). It covers this repository's code only and grants no rights to any game assets.
- `slugcatpet/audio/fsbogg.py` is partly adapted from [python-fsb5](https://github.com/HearthSim/python-fsb5)
  (MIT, Simon Pinfold); the data in `vorbis_setups.py` was generated with libvorbis (BSD, Xiph.Org).

### Upstream and version

- Upstream: [lingxiaojun/slugcatpet](https://github.com/lingxiaojun/slugcatpet)
- This fork is based on upstream **v1.3.4** (`0853bdb`), merge commit `9053500`
- Upstream demo videos: [Bilibili collection](https://space.bilibili.com/1810584454/lists?sid=8504199)
- Local changes focus on the native Wayland/KWin port and Linux desktop integration. Upstream releases are merged regularly.

If you only use Windows and do not need this fork's Wayland or local features, the upstream release is
usually the simpler option. Report Wayland issues here; report upstream gameplay issues upstream.
