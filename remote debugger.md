1. 配置SSH免密登录
    ``` shell
    # 生成SSH密钥（本地），默认地址：~\<用户名>\.ssh\，可不设密码
    ssh-keygen -t ed25519 -C "your_email@example.com"

    # 将id_ed25519.pub内容复制到远程服务器
    mkdir -p ~/.ssh
    echo "id_ed25519.pub content" >> ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/authorized_keys  # 必须限制权限

    # 测试免密登录
    ssh user@remote_erver_ip
    ```
2. 配置VS Code
   1. 安装 Romote SSH 插件
   2. 点击VS Code左下角 **><** 图标 》**Connect to Host...**
   3. 输入 ssh user@remote_server_ip
   4. 首次连接选择服务器平台（Linux）
3. 远程开发环境配置
   ``` bash
   sudo apt update && sudo apt install y gcc g++ gdb make cmake
   ```
4. 在VS Code中打开远程文件夹
5. 编译与调试配置
   1. 配置tasks.json（编译）
        按 **Ctrl+Shift+P**，输入 **Tasks:Configure Task**，选择 **Create tasks.json file from template**
        ``` json
        {
            "version": "2.0.0",
            "tasks": [
                {
                    "label": "build busywait",
                    "type": "shell",
                    "command": "g++",
                    "args": [
                        "-std=c++17",
                        "-O0",
                        "-g",
                        "-pthread",
                        "-march=native",
                        "-o",
                        "busywait",
                        "main.cpp"
                    ],
                    "group": "build",
                    "problemMatcher": []
                }
            ]
        }
        ``` 
   2. 配置launch.json（调试）
        按**Ctrl+Shift+D**，点击 **create a launch.sjon file**，选择 **C/C++:(gdb)Launch**
        ``` json
        {
            // Use IntelliSense to learn about possible attributes.
            // Hover to view descriptions of existing attributes.
            // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
            "version": "0.2.0",
            "configurations": [
                {
                    "name": "(gdb) Launch busywait",
                    "type": "cppdbg",
                    "request": "launch",
                    "program": "${workspaceFolder}/busywait",
                    "args": ["100", "60", "1"],
                    "stopAtEntry": false,
                    "cwd": "${workspaceFolder}",
                    "environment": [],
                    "externalConsole": false,
                    "MIMode": "gdb",
                    "miDebuggerPath": "/usr/bin/gdb",
                    "preLaunchTask": "build busywait",
                    "setupCommands": [
                        {
                            "description": "Enable pretty-printing for gdb",
                            "text": "-enable-pretty-printing",
                            "ignoreFailures": true
                        }
                    ]
                }

            ]
        }
        ```
   3. 1
