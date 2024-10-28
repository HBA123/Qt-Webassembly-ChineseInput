# Qt for Webassembly的中文输入
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

## 安装中文字体
首先需要下载中文字体库（.ttf格式）。由于需要用于网络传输，其大小不能太大，最好在几MB左右
