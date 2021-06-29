# help（获取帮助信息）

help命令用来获取命令的帮助信息，当您不清楚某个命令的用法时，建议您使用help命令获取该命令的帮助信息。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil help [command]
```

## 使用示例

-   获取所有命令帮助信息

    ```
    ./ossutil help
    ```

-   获取cp命令帮助信息

    ```
    ./ossutil help cp
    ```

-   查看所有选项信息

    ```
    ./ossutil help -h
    ```

-   切换为中文显示ls命令的帮助

    ```
    ./ossutil help ls -L ch
    ```


