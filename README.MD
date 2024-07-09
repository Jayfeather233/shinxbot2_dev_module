# shinxBot2-dev-module

[shinxbot2](https://github.com/Jayfeather233/shinxbot2)的功能开发最小单元。

开发前请确保能进行shinxbot2的编译。

### 添加功能

functions/events 文件夹里有一个基本示范。

1. ./src/functions 下新建目录并把代码放入，写一个类继承 `./src/interfaces/processable.h` 中的类并实现virtual方法，并提供`create`方法得到功能`processable*`的指针（可以参考`functions/recall/`）

2. 继承的 virtual 方法里只要 check() 成功了就会执行 process() 方法

3. 在`generate_cmake.py`中有CMake模版，用其编译你的功能，自动生成动态链接库至`./lib/functions/`

4. 将其复制至bot的lib里。

5. 启动bot。如果bot正在运行可以使用管理员账号（`op_list.json`）发送`bot.load [function|event] name` 加载。
