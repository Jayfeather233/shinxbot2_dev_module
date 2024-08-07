# shinxBot2-dev-module

[shinxbot2](https://github.com/Jayfeather233/shinxbot2)的功能开发最小单元。

这个库不会与原库保持同步，用此库代码编译出的功能也不保证能在bot上运行，只是作为一个最小单元的示范存在。不过一般来说，只需将原shinxbot2仓库 头文件/libutils.so 替换本库 头文件/libutils.so，就能正常编译使用。

开发前请确保能进行shinxbot2的编译。

### 添加功能

./src/Yourfunc 文件夹里有一个基本示范。

1. 写一个类继承 `./src/shinxbot_api/processable.h`/`./src/shinxbot_api/eventprocess.h` 中的类并实现virtual方法，并提供`create`方法得到功能`processable*`/`eventprocess*`的指针

2. 继承的 virtual 方法里只要 check() 成功了就会执行 process() 方法

3. 用`./build.sh`编译你的功能，自动生成动态链接库至`./build/`（可能需要对CMakeLists.txt进行一些更改，希望你会使用CMake :D）

4. 将其复制至bot的lib里。

5. 启动bot。如果bot正在运行可以使用管理员账号（`op_list.json`）发送`bot.load [function|event] name` 加载。

bot加载库时，会调用 `extern "C" processable *create()` 来得到功能指针。一般来说直接返回 `new YourFunc()` 即可。

bot卸载库时，会调用 `extern "C" processable *close(processable* p)` 来表明需要卸载库。请合理释放所有全局 `new` 得到的指针，包括参数 `p`。
