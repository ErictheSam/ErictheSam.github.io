---
layout: post
title: 本人的vscode配置
---

我在升级了系统重新配置vscode的时候，使用了如下的顺序来进行配置，从而使得自己的工作效率达到较大的值。

首先是扩展部分：我按照顺序和操作逻辑下载了如下的插件

* C/C++，C++ Intellisence——C++配置
* python, pylance——python配置
* Git History，Gitlens——git配置
* Markdown Preview Enhanced——markdown（说明文档）配置
* Run in Save, Auto Snippet——综合配置
* Bazel——编译用
* 外界包相关，例如proto，见到一个安装一个插件

然后是各项配置，我按照从综合到具体的顺序进行了如下的操作

* 对于git，我首先设置了不auto fresh(settings.json)，并且设置了一些keybind。

  * 设置方法如下：

    ```json
    {
        "git.autorefresh": false,
    }
    ```

    settings.json

    ```json
    [
        {
            "key": "shift+alt+r",
            "command": "-remote-wsl.revealInExplorer",
            "when": "false"
        },
        {
            "key": "shift+alt+r",
            "command": "git.refresh"
        }
    ]
    ```

    keybindings.json

* 对于general的文件过滤，我在具体workspace下进行了如下的配置：

  * 在workspace的.vscode文件夹中，找到`settings.json`，在"files.exclude"一栏里面列出可能遇到的各种file和folder。

* 对于C++，我进行了如下的几处配置：

  * 下载了clang-format-8，然后在`settings.json`里面做了如下配置：

    ```json
    "[cpp]": {
        "editor.formatOnSave": true,
        "editor.formatOnSaveMode": "modifications",
    },
    ```

    

  * 关于包的管理，两个东西比较有用：vcpkg和global

    * global包的作用是导航workspace内部的东西，具体安装结束之后在workspace下执行命令

      ```bash
      sudo gtags -v
      ```

      生成所有的tags

    * vcpkg是外界包的管理器，各种.h文件的显式可以在这里生成；只要通过import failure的指示，调用

      ```bash
      ./vcpkg install xxx
      ```

      然后在`settings.json`里面的`C_Cpp.default.includePath`选项里增加`${vcpkg_place}/packages/***`

  * 安装了C++ intellisense的情况下，我们要做这两个工作：

    * 1. 修改权限，把`~/.vscode/extensions/austin.code-gnu-global-0.2.2/out/src/features/referenceProvider.js`里的`-rax`改成`-rsax`
      2. 就是调用上面的`gtags`相关命令。

* 对于python，我做了如下的设置：

  * 安装yapf和pylint，然后在user的`settings.json`里面加上如下设置：s

    ```json
    // pylint
    "python.linting.enabled": true,
    "python.linting.pylintEnabled": true,
    "python.linting.pylintArgs": [
        "--rcfile=${workspaceFolder}/.pylintrc"
    ],
    // yapf
    "python.formatting.provider": "yapf",
    "python.formatting.yapfArgs": [
        "--style", "${workspaceFolder}/.yapf-styles.conf"
    ],
     
    // uncomment it to run yapf on save. Be careful it will run yapf for any opened python files.
    // "editor.formatOnSave": true,
    ```

    .pylintrc按照所需的工程规则去配置，.yapf-styles.conf可以按照如下的谷歌规范配置：

    ```
    [style]
    based_on_style = google
    column_limit = 100
    allow_multiline_lambdas = True
    ```
