# Qt for Webassembly的中文输入
本人环境
* windows11操作系统
* Qt版本6.8.0，QtCreator版本14.0.2

由于wasm技术是基于浏览器显示的，突出一个跨平台技术，故可以就在win环境下编写、编译、打包，之后在Linux操作系统（有浏览器）下也可以运行

## Emscripten的安装


源码下载：
  
    git clone https://github.com/emscripten-core/emsdk

cd到下载的源码的根目录下`cd emsdk`
    
顺序输入如下命令，进行激活、设置到环境变量中:
```
emsdk update #更新
emsdk install --global 3.1.56 # emsdk的版本要与Qt版本对应
emsdk activate 3.1.56 # 激活
emsdk_env.bat # 设置到环境变量中
```
查看Qt与emsdk版本对应的[网站](https://doc.qt.io/qt-6/wasm.html#fonts)  
![image](https://github.com/user-attachments/assets/5fb01448-a6d4-4aab-94fa-694f600f22a9)  
我在windows上安装的Qt是6.8.0，故选择安装与激活的emsdk的版本为3.1.56


之后输入`emcc -v`命令查看，如果有下面类似的输出说明安装正确：
```
emcc (Emscripten gcc/clang-like replacement + linker emulating GNU ld) 3.1.56 (cf90417346b78455089e64eb909d71d091ecc055)
clang version 19.0.0git (https:/github.com/llvm/llvm-project 34ba90745fa55777436a2429a51a3799c83c6d4c)
Target: wasm32-unknown-emscripten
Thread model: posix
InstalledDir: E:\emsdk-main\upstream\bin
```
## QtCreator配置
提前安装好Python，最新版的就行，记得配置好系统环境变量  
我安装的Python是3.9版本，如果最新版报错可以删除后下载低版本的  

在`编辑`->`preferences`->`设备`->`WebAssembly`路径下，点击`浏览`导入`emsdk-main`根目录，点击`应用`与`确定`就行了

## 安装中文字体
首先需要下载中文字体库（.ttf或.otf格式）。由于需要用于网络传输，其大小不能太大，最好在几MB左右（太大的字体库不能加载，巨坑）  
可以使用我上面提供的Tiny字体库中的一个  

在创建的项目中，右键添加新文件，选择Qt Resource File添加资源文件（如果项目中已经有了.qrc文件可以略去这一步）：
![image](https://github.com/user-attachments/assets/bd73b503-a31d-42ba-aa31-843b708a8f96)

之后，将选择的ttf字体库文件拷贝到项目相同的根目录下，即与`main.cpp`等文件同一级，确保绝对路径或者相对路径没有中文  
在QtCreator中，右键.qrc文件，点击添加现有文件，选择.ttf文件  

在`main.cpp`中，添加如下代码：
```cpp
    int fontId = QFontDatabase::addApplicationFont(QStringLiteral(":/hei.TTF"));  // 此处索引到你添加的.ttf文件
    QStringList fontFamilies = QFontDatabase::applicationFontFamilies(fontId);
    if (fontFamilies.size() > 0) {
        QFont font;
        font.setFamily(fontFamilies[0]);
        a.setFont(font);
    }
```
记得在`main.cpp`中添加头文件：
```cpp
#include <QFontDatabase>
```

之后修改`CmakeLists.txt`文件，添加代码：

    set(CMAKE_AUTORCC ON)
  
还是在`CmakeLists.txt`文件中，在`set(PROJECT_SOURCES ... )`里面添加之前新建的.qrc文件，比如：
```
set(PROJECT_SOURCES
        main.cpp
        mainwindow.cpp
        mainwindow.h
        mainwindow.ui
        # ${TS_FILES}
        font.qrc
)
```
修改完后记得保存  
之后编译webassembly应用，即可在浏览器显示、输入中文
