# Simple OpenCV demo

## Build

Don't forget to clone the submodules

```bash
git submodule update --init --recursive
```

### Build in Windows

When I say "install" I also mean "add to your `PATH` environment variable".

- Install [Ninja](https://ninja-build.org/). Extract the zip file and add the
  folder to your `PATH` environment variable.
- Install [CMake](https://cmake.org/download/) by Installer.
- Install [LLVM](https://github.com/llvm/llvm-project/releases). Find Windows
installer called `LLVM-<version>.win64.exe`. (like [`LLVM-17.0.1-win64.exe`](https://github.com/llvm/llvm-project/releases/download/llvmorg-17.0.1/LLVM-17.0.1-win64.exe))
I assume you install it to `C:\Program Files\LLVM` so clang should be `C:\Program Files\LLVM\bin\clang.exe`.
- Install [openCV binary from release](https://opencv.org/releases/). (like [`opencv-4.8.0-windows.exe`](https://github.com/opencv/opencv/releases/download/4.8.0/opencv-4.8.0-windows.exe)). See [OpenCV Windows Installation](#opencv-windows-installation).

You DO NOT NEED to install [Visual Studio](https://visualstudio.microsoft.com/downloads/) (?) since we are using LLVM and Windows SDK is not really needed. (Not sure, if problems occur, try to install it.)

You DO NOT NEED [MSYS2](https://www.msys2.org/) since no [MingGW](https://www.mingw-w64.org/) should be used. 

You could still try
[`mingw-w64-x86_64-opencv`](https://packages.msys2.org/package/mingw-w64-x86_64-opencv)
or
[`mingw-w64-ucrt-x86_64-opencv`](https://packages.msys2.org/package/mingw-w64-ucrt-x86_64-opencv)
in MSYS2 in a MinGW environment. I guess you have to use `gcc` instead of `clang` to build it.

#### OpenCV Windows Installation

Run the executable and extract the files to a folder. I prefer `C:\opt\opencv`. 

```txt
opencv
├── build
├── LICENSE_FFMPEG.txt
├── LICENSE.txt
├── README.md.txt
└── sources
```

Add `C:\opt\opencv\build\x64\vc16\bin` to your `PATH` environment variable. `PATH` acts like `LD_LIBRARY_PATH` in Linux here
so missing this step will cause `dll not found` error. In my machine even no error was raised so I have to check the dynamic linking manually using [lucasg/Dependencies](https://github.com/lucasg/Dependencies).

See also

- [MSYS2 Environments](https://www.msys2.org/docs/environments/)
- [CMakeLists for OpenCV that installed using Vcpkg ](https://gist.github.com/UnaNancyOwen/5061d8c966178b753447e8a9f9ac8cf1)
- [Use Windows Terminal with MSYS2](https://www.msys2.org/docs/terminals/)
- [lucasg/Dependencies](https://github.com/lucasg/Dependencies) An open-source modern Dependency Walker (like `ldd` in Linux)
- [libc++ 与 libstdc++ 是什么关系？](https://www.zhihu.com/question/343205052)
- [C/C++标准库](https://zhuanlan.zhihu.com/p/566419668)

#### Build walkthrough

```powershell
mkdir build
cd build
cmake -DOpenCV_DIR=C:/opt/opencv/build/x64/vc16/lib -DCMAKE_CXX_COMPILER="C:/Program Files/LLVM/bin/clang++.exe" -DCMAKE_C_COMPILER="C:/Program Files/LLVM/bin/clang.exe" -G Ninja ..
ninja -j (Invoke-Expression [Environment]::ProcessorCount)
```

#### Visual Studio Code

See [`.vsode/settings.windows.jsonc`](./.vscode/settings.windows.jsonc). Change it to `.vscode/settings.json` to be your workspace configuration.

### Build in Linux

#### Install dependencies

##### Ubuntu/Debian

```bash
sudo apt install build-essential cmake ninja-build
sudo apt install libopencv-dev
```

##### Arch Linux

```bash
pacman -S base-devel cmake ninja
pacman -S opencv
```

##### Build

This is a typical out-of-source build. Trivially, you can run the following commands:

```bash
mkdir build
cd build
cmake .. -G Ninja
ninja -j$(nproc)
```
