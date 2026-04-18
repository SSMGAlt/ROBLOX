# Roblox

This is a fixed and partially extended version of the Roblox source code from 2016. The original source was obtained from [git.rip](https://git.rip/exconfidential/roblox/roblox) (now taken down).

[![Dependabot Updates](https://github.com/SSMGAlt/ROBLOX/actions/workflows/dependabot/dependabot-updates/badge.svg)](https://github.com/SSMGAlt/ROBLOX/actions/workflows/dependabot/dependabot-updates)
[![CodeQL](https://github.com/SSMGAlt/ROBLOX/actions/workflows/codeql.yml/badge.svg)](https://github.com/SSMGAlt/ROBLOX/actions/workflows/codeql.yml)

[![Star History Chart](https://api.star-history.com/svg?repos=SSMGAlt/ROBLOX&type=Date)](https://star-history.com/#SSMGAlt/ROBLOX&Date)

---

## Notes

Pull requests are accepted for bug fixes, dependency updates, and general improvements.

If the build fails or there are any issues with image assets (`.png`, `.svg`, etc.), please open an [Issue](https://github.com/SSMGAlt/ROBLOX/issues). If you are unable to submit a pull request, a fix will be applied directly.

---

## Building

### Windows

**Prerequisites:**
- Visual Studio 2019
- Visual Studio 2015 Build Tools (toolset `v141` — `v141_xp` may also work but has not been confirmed)

#### Step 1 — Build Boost

1. Navigate to `Library/boost/` and run `bootstrap.bat`.
2. Open `build_boost.bat` and update the paths to match the location of this repository on your machine.
3. Run `build_boost.bat`.

Boost is now built.

#### Step 2 — Build Qt

Qt requires the Visual Studio 2015 Build Tools. The steps below use the VS2015 x86 Native Tools Command Prompt.

1. Open the **VS2015 x86 Native Tools Command Prompt** (search "VS2015" in the Start Menu).
2. Change your working directory to `Library/Qt/` within this repository.
3. Run the `configure` command below, replacing `${path}` with the absolute path to the root of this repository:

```sh
./configure -make nmake -platform win32-msvc2015 -prefix ${path}\Library\Qt -opensource -confirm-license -opengl desktop -nomake examples -nomake tests -webkit -xmlpatterns
```

For example, if the repository is located at `D:\Roblox\Source`:

```sh
./configure -make nmake -platform win32-msvc2015 -prefix D:\Roblox\Source\Library\Qt -opensource -confirm-license -opengl desktop -nomake examples -nomake tests -webkit -xmlpatterns
```

4. Once configuration is complete, run `nmake`.
   - If you receive an error stating that `rc` is not recognized, add your Windows SDK `bin` folder to your `PATH` environment variable.
5. The build will eventually stop with an error, but all required Qt libraries will have been produced by that point.

Qt is now partially compiled (only the libraries needed for this project are required).

#### Step 3 — Build Roblox Projects

Open `Roblox.sln` in Visual Studio 2019 and build the solution. All projects that are currently supported will compile successfully. Refer to the [project status list](#project-status) below for details on which projects are supported.

---

### macOS

**Prerequisites:**
- Xcode (latest version recommended)
- Xcode Command Line Tools (`xcode-select --install`)

> **Note:** The macOS client (`MacClient` and `RobloxMac`) does not currently compile successfully. The instructions below describe how to attempt a build.

1. Open `Mac/Mac.xcworkspace` in Xcode.
2. Select the target scheme (`MacClient` or `RobloxMac`) from the scheme selector.
3. Build the project using **Product > Build** or `Cmd+B`.

If you encounter missing dependency errors, ensure that the required libraries in `Library/` have been built or are present. Contributions toward making the Mac build functional are welcome.

---

### Android

**Prerequisites:**
- Android Studio (latest version recommended)
- Android NDK (r21 or later recommended)
- CMake 3.10 or later

> **Note:** The Android client does not currently compile successfully. The instructions below describe how to attempt a build.

1. Open Android Studio and select **Open an Existing Project**.
2. Navigate to the `Android/` directory within this repository and open it.
3. Android Studio will prompt you to configure the NDK. Set the NDK path in **File > Project Structure > SDK Location**.
4. Sync the Gradle project and attempt to build via **Build > Make Project**.

Alternatively, you can use the CMake build system directly:

```sh
cmake -DCMAKE_TOOLCHAIN_FILE=${NDK_PATH}/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=armeabi-v7a \
      -DANDROID_PLATFORM=android-21 \
      -B build/android
cmake --build build/android
```

Replace `${NDK_PATH}` with the path to your Android NDK installation.

---

### iOS

**Prerequisites:**
- macOS with Xcode installed
- A valid Apple Developer account (required for device deployment; simulator builds may work without one)

> **Note:** The iOS client does not currently compile successfully. The instructions below describe how to attempt a build.

1. Open `Mac/Mac.xcworkspace` or `MacClient.xcodeproj` in Xcode.
2. Select the `iOS` target scheme from the scheme selector.
3. Choose a simulator or connected device as the build destination.
4. Build via **Product > Build** or `Cmd+B`.

You may need to configure a development team under **Signing & Capabilities** in the project settings if targeting a physical device.

---

## Libraries

Library names marked with an asterisk (\*) are out of date and may require updating.

| Library | Version |
|---|---|
| Boost | 1.74.0 |
| libcurl | 7.71.0 |
| zlib | 1.12.11 |
| SDL | 2.0.12 |
| VMProtect \* | 2.1.3 |
| cpp-netlib | 0.13.0-final |
| Mesa \* | 7.8.1 |
| xulrunner-sdk \* | 1.9.0.11 (en-US, win32) |
| glsl-optimizer \* | — |
| hlsl2glsl \* | — |
| cabsdk \* | — |
| Windows/DirectX SDK | — |
| w3c-libwww | 5.4.2 |
| Qt \* | 4.8.5 |

---

## Project Status

The following is a comprehensive list of all projects and their current build status, as well as planned and completed changes to the original source code.

### Compilable Projects

- [x] App
- [x] AppDraw
- [x] Network
- [x] RCCService
- [x] Base
- [x] boost.static
- [x] boost.test
- [x] GfxBase
- [x] RbxG3D
- [x] graphics3D
- [x] RbxTestHooks
- [ ] Base.UnitTest
- [ ] App.UnitTest
- [x] RobloxStudio
- [x] Log
- [x] WindowsClient
- [ ] RobloxTest
- [x] GfxCore
- [x] GfxRender
- [x] CSG
- [x] App.BulletPhysics
- [ ] CoreScriptConverter2
- [ ] Microsoft.Xbox.GameChat
- [ ] Microsoft.Xbox.Samples.NetworkMesh
- [ ] XboxClient
- [ ] Bootstrapper
- [ ] BootstrapperClient
- [x] RobloxProxy
- [x] NPRobloxProxy
- [ ] BootstrapperQTStudio
- [ ] BootstrapperRCCService
- [ ] RCCServiceArbiter
- [x] RobloxModelAnalyzer
- [ ] Extract RbxDebug source files from bin/obj files
- [ ] MacClient
- [ ] RobloxMac
- [ ] Android
- [ ] iOS

### Backported Features

- [ ] Studio dark theme
- [ ] Constraints
- [ ] Sound.PlaybackSpeed
- [ ] ScreenGui.Enabled
- [ ] ScreenGui.DisplayOrder
- [ ] Atmosphere
- [ ] Instance
  - [x] Wait
  - [x] Connect
  - [ ] GetPropertyChangedSignal
- [ ] AnimationTrack.Looped
- [ ] Color3uint8
- [x] Sky
  - [x] SunAngularSize
  - [x] MoonAngularSize
  - [x] SunTextureId
  - [x] MoonTextureId
- [x] Lighting
  - [x] ClockTime
- [ ] Post Processing
  - [x] MSAA
    - [ ] AASamples
  - [ ] BloomEffect
  - [ ] BlurEffect
  - [ ] ColorCorrectionEffect
  - [ ] SunRaysEffect
  - [ ] Attempt a backport of the 2015 1x1 stud voxel shadow FIB prototype (demo [here in 2015](https://www.youtube.com/watch?v=z5TmqDtpwSM) and [here in 2014](https://www.youtube.com/watch?v=Y9-KDzMasjg))
- [ ] TextSize
- [x] Easier to read debug stat GUIs
- [ ] LayerCollector.ResetOnSpawn
- [x] "Oof" sound in volume slider
- [ ] Smooth camera scrolling
- [ ] Color3
  - [x] Color3.fromRGB
  - [ ] Color3.fromHSV
  - [ ] Color3.toHSV
- [ ] Instance:GetDescendants()
- [ ] MeshPart
- [ ] Terrain
  - [ ] WaterReflectance
  - [ ] MaterialColors
  - [ ] New 2020 materials
- [ ] Sound effects
  - [ ] FlangeSoundEffect
  - [ ] SoundEffect
  - [ ] SoundGroup
  - [ ] PitchShiftSoundEffect
  - [ ] ChorusSoundEffect
  - [ ] CompressorSoundEffect
  - [ ] TremoloSoundEffect
  - [ ] ReverbSoundEffect
  - [ ] DistortionSoundEffect
  - [ ] EchoSoundEffect
  - [ ] EqualizerSoundEffect
- [ ] ScreenGui.IgnoreGuiInset
- [ ] TextBox
  - [ ] TextBox.CursorPosition
  - [ ] TextBox.SelectionStart
  - [ ] Home/End key support
- [ ] New Developer Console
- [ ] Team
  - [ ] Player.Team
  - [ ] Team:GetPlayers()
  - [ ] Team.PlayerAdded
  - [ ] Team.PlayerRemoved
- [ ] Particle visibility
  - [ ] ForceField.Visible
  - [ ] Explosion.Visible
- [ ] Decal.Color3
- [ ] ClickDetector
  - [ ] ClickDetector.RightMouseClick
  - [ ] ClickDetector.CursorIcon
- [ ] HttpService headers in HttpService:GetAsync and HttpService:PostAsync
- [ ] Humanoid.FloorMaterial

### New Features

- [x] WaterWaveSize and WaterWaveSpeed can be in the range of `-FLT_MAX` to `FLT_MAX`
- [ ] TextBox text selection
- [ ] Restore SafeChat (Studio Settings > Game Options > ShowSafeChatButton)
- [ ] `int64` support
- [ ] Unicode/UTF-8 support
- [ ] 64-bit support
- [ ] Compile with VC++ 2019
- [x] Uncapped friction
- [x] Lua 5.3 utf8 library port
- [ ] Unlock TextureTrail
- [x] Uncapped PlayerGui:SetTopbarTransparency
- [ ] Mesh format version 3.00 support
- [ ] Color3.toRGB

### Bug Fixes

- [ ] DirectX
  - [x] DirectX 9 text render bug (fixed; determined to have never existed in the original source)
  - [ ] DirectX 11 not initializing
- [x] SDL Windows key
- [x] Keyboard shortcuts
- [x] Chat output shrinking on window minimize
