# cpp-catch2-demo
The Catch2 demo help to test your c++ code.


### C/C++ 開發工具與環境配置

:::info
- OS          : Windows10 ver.20H2
- IDE         : VSCode
- C/C++ Tools : Mingw-w64、Cmake、vcpkg、MSBuild
:::

Windows 環境變數配置
---

|PATH|ICON|
|-|-|
|D:\mingw64\bin\ |![](https://i.imgur.com/l2GKkZs.png)|
|D:\CMake\bin\ |![](https://i.imgur.com/Rfy6TJr.png)
|D:\vcpkg\ |![](https://i.imgur.com/ID13RdW.png)|
|C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\MSBuild\Current\Bin\ |![](https://i.imgur.com/pHBX6SY.png) MSBuild


[MinGW-w64]((https://sourceforge.net/projects/mingw-w64/))
---

MinGW-w64 是一個完整的 gcc runtime，用於支持 Windows 64 位和 32 位操作系統的原生二進製文件。由於個人電腦為Windows，所以我採用 ```MinGW-W64 GCC-8.1.0 [x86_64-posix-seh]``` ，經過本人測試，確定可以使用於 win10-20H2



[Vcpkg](https://vcpkg.io/en/getting-started.html)
---

vcpkg 為 C/C++函式庫(libraries)管理工具。在vscode時需要一些配置。

### 下載安裝vcpkg

可以透過git clone或是.zip檔載解壓縮即可!

```
git clone https://github.com/Microsoft/vcpkg.git
```
首次啟動必須先到下載好的vcpkg目錄下，執行```.\vcpkg\bootstrap-vcpkg.bat``` 有新增好路徑，就可以使用vcpkg指令了。

- VCPKG_DEFAULT_TRIPLET
使用上，可設置環境變數 ```VCPKG_DEFAULT_TRIPLET``` 其值設置為 ```x64-windows```，能避免Vcpkg下載到32-bit版本的包

```
VCPKG_DEFAULT_TRIPLET=x64-windows
```

VSCode
---

### 下載 Extension : [C/C++] From Microsoft
![](https://i.imgur.com/PDxwTQW.png)

### 配置 vscode user settings

- CMAKE_TOOLCHAIN_FILE
仰賴CMake來幫我們找到他們，如使用find_package()功能。

- C_Cpp.default.includePath
這是vcpkg會幫我們管理下載下來的第三方庫，以```x64-windows```的包為例，就會被放在```vcpkg\installed\x64-windows\include\```下，而這裡配置便是要提供vscode資訊來如找到 c/c++庫。

```json=
{
  "cmake.configureOnOpen": false,

  "cmake.configureSettings": {
    "CMAKE_TOOLCHAIN_FILE":"D:/vcpkg/scripts/buildsystems/vcpkg.cmake"
  },
  "C_Cpp.default.includePath": [
    "D:\\vcpkg\\installed\\x64-windows\\include"
  ]
}
```

[CMake](https://cmake.org/download/)
---

CMake 可以生成跨平台通用的專案腳本CMakeLists.txt。原理上CMake根據CMakeLists.txt在當前平台才開始編譯，並產生對應當前平台的工具鏈，若為 Windows 64 位元的版本，至官網上擇一下載安裝即可。

|Platform | Files|
|---------|------|
|Windows x64 installer|cmake-3.23.1-windows-x86_64.msi|
|Windows x64 ZIP|cmake-3.23.1-windows-x86_64.zip|

### CMake Vscode Extension
- CMake
- CMake Tools

### CMake Generator

我們必須根據當前平台，來決定怎麼生成 CMakeLists.txt 文件，比較便捷確實的作法就是使用 CMake Generator，這裡我們使用 [Visual Studio Tools](https://visualstudio.microsoft.com/zh-hant/downloads/?q=build+tools) 來構建。首先，先安裝下載。

### CMake Scan

回到Vscode中，我們可以```ctrl+p```，若是新的專案，輸入```CMake:Quick Start```，把```menu```叫出來，透過上面的```scan for kits```，它會重新搜尋你擁有的generators。若發現你的面板新增出現你剛剛安裝的套件，代表成功!

### CMake Build

```ctrl+p```，輸入```CMake:Build```


Catch2
---

Catch2 is mainly a unit testing framework for C++, but it also provides basic micro-benchmarking features, and simple BDD macros.

### 透過vcpkg下載Catch2

```
vcpkg install Catch2:x64-windows
```

### 添加以下代碼到 CMakeLists.txt

```txt=

find_package(Catch2 CONFIG REQUIRED)
target_link_libraries(cpp-catch2-demo PRIVATE Catch2::Catch2)

add_custom_target(run
  COMMAND ${PROJECT_NAME}
  DEPENDS ${PROJECT_NAME}
  WORKING_DIRECTORY ${CMAKE_PROJECT_DIR}
)
```

### 運行 Catch2

因為沒有RUN的功能，所以就自己建一個。在 CMakeLists.txt 再添加如下代碼

```txt=
add_custom_target(run
  COMMAND ${PROJECT_NAME}
  DEPENDS ${PROJECT_NAME}
  WORKING_DIRECTORY ${CMAKE_PROJECT_DIR}
)
```
在vscode輸入快捷鍵 ```ctrl + p```；```cmake:build target```；```run```

![](https://i.imgur.com/1NHdwEX.png)
![](https://i.imgur.com/N0S21wd.gif)

另外一個做法便是透過下方的狀態列直接去執行。執行Build後，它會自動將服務的控制權轉移到新的Terminal，讓你可以去控制。

![](https://i.imgur.com/yrqShXP.png)
