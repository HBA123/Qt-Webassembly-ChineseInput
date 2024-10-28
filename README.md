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
emsdk install --global latest # 安装
emsdk activate latest # 激活
emsdk_env.bat # 设置到环境变量中
```

之后输入`emcc -v`命令查看，如果有下面类似的输出说明安装正确：
```
emcc (Emscripten gcc/clang-like replacement + linker emulating GNU ld) 3.1.56 (cf90417346b78455089e64eb909d71d091ecc055)
clang version 19.0.0git (https:/github.com/llvm/llvm-project 34ba90745fa55777436a2429a51a3799c83c6d4c)
Target: wasm32-unknown-emscripten
Thread model: posix
InstalledDir: E:\emsdk-main\upstream\bin
```
## QtCreator配置
提前安装好Python，最新版的就行  

在`编辑`->`preferences`->`设备`->`WebAssembly`路径下，点击`浏览`导入`emsdk-main`根目录，点击`应用`与`确定`就行了

## 安装中文字体
首先需要下载中文字体库（.ttf或.otf格式）。由于需要用于网络传输，其大小不能太大，最好在几MB左右（太大的字体库不能加载，巨坑）  
可以使用我上面提供的Tiny字体库中的一个  

在创建的项目中，右键添加新文件，选择Qt Resource File进行添加：
![image](https://github.com/user-attachments/assets/bd73b503-a31d-42ba-aa31-843b708a8f96)

之后，将选择的ttf字体库文件拷贝到项目相同的根目录下，即与main.cpp等文件同一级，确保绝对路径或者相对路径没有中文  
在QtCreator中，右键.qrc文件，点击添加现有文件，选择.ttf文件  

在main.cpp中，添加如下代码：
```cpp
    int fontId = QFontDatabase::addApplicationFont(QStringLiteral(":/hei.TTF"));
    QStringList fontFamilies = QFontDatabase::applicationFontFamilies(fontId);
    if (fontFamilies.size() > 0) {
        QFont font;
        font.setFamily(fontFamilies[0]);
        a.setFont(font);
    }
```
记得添加头文件：
```cpp
#include <QFontDatabase>
```

之后编译webassembly应用，即可在浏览器显示、输入中文
