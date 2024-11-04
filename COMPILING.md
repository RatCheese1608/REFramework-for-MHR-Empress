# Compiling REFramework

## Necessary prerequisites

A C++23 compatible compiler is required. Visual Studio 2022 is recommended. Compilers other than MSVC have not been tested.

CMake is required.

NOTE: If you don't have Visual Studio, you can just download the build tools from their download page. Install MSVC, CMake, and Windows SDK. The download size is pretty large but other methods are too much of a bother. Msys2's CMake had many issues when I tried it.

## Compiling

###  Clone the repository

#### SSH
```
git clone git@github.com:praydog/REFramework.git
```

#### HTTPS
```
git clone https://github.com/praydog/REFramework
```

### Initialize the submodules

```
git submodule update --init --recursive
```

### Set up CMake

#### Command line

```
cmake -S . -B build ./build -G "Visual Studio 17 2022" -A x64 -DCMAKE_BUILD_TYPE=Release
cmake --build ./build --config Release
```

NOTE: Avoid using msys2's python I think using msys2's python interpretor affects the results (originally it kept producing lib instead of dll).

Without adding `-DPYTHON_EXECUTABLE:FILEPATH`, cmake kept defaulting to use python3.exe from my Msys2 installation.

Another thing I'd like to add is that after the first command, a "build" folder will be made. If you've already run the first command, it's likely that you won't need to run it again. You can just modify the .cpp files and then run the second command. If you're worried that you'll have to constantly build from scratch, the second command utilizes using prebuilt dependencies so it'll be faster on your second build. There's a ton of files that get compiled into the dll, so the build time takes a lot of memory and time. It took 4Gigs of RAM during my process.

This was the command I used:

```
cmake -S . -B build ./build -G "Visual Studio 17 2022" -A x64 -DCMAKE_BUILD_TYPE=Release -DDEVELOPER_MODE=OFF -DREF_BUILD_MHRISE_SDK=ON -DPYTHON_EXECUTABLE:FILEPATH="C:\path\to\Python\Python311\python.exe"
cmake --build . --config Release --target MHRISE
```

Keep in mind that not supplying a target will build multiple targets, when you may only need one of them.

For example, to build only the RE2 target, you can use the following command:

```
cmake --build ./build --config Release --target MHRISE
```

#### VSCode

1. Install the [CMake Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools) extension
2. Open the REFramework folder in VSCode
3. Press `Ctrl+Shift+P` and select `CMake: Configure`
4. When "Select a kit" appears, select `Visual Studio Community 2022 Release - amd64`
5. Select the desired build config (usually `Release` or `RelWithDebInfo`) near the bottom of the window
6. Select the desired build target near the bottom of the window, otherwise all targets will be built
6. You should now be able to compile REFramework by pressing `Ctrl+Shift+P` and selecting `CMake: Build` or by pressing `F7`

#### Batch script

In the root of the repository there is a `build_vs2022.bat` script that will build all targets in Release mode.
