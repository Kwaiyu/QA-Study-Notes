## 异常报错

**IDEA创建Maven项目添加依赖编译运行提示xxx程序包不存在**

settings > file encodings 全部UTF-8

file invalidate caches

java compiler 14，14

maven importing 14

project structure > project 14,14 

project structure > modules sources 14，dependencies 14

## 0x1. 如何安装

### 1). 插件市场安装：

- 在`Settings/Preferences...` -> `Plugins` 内手动添加第三方插件仓库地址：`https://plugins.zhile.io`
- 搜索：`IDE Eval Reset`插件进行安装。如果搜索不到请注意是否做好了上一步？网络是否通畅？
- 插件会提示安装成功。

### 2). 下载安装：

- 点击这个[链接(v2.2.4)](https://plugins.zhile.io/files/ide-eval-resetter-2.2.4-4959c6.zip)下载插件的`zip`包（macOS可能会自动解压，然后把`zip`包丢进回收站）
- 通常可以直接把`zip`包拖进IDE的窗口来进行插件的安装。如果无法拖动安装，你可以在`Settings/Preferences...` -> `Plugins` 里手动安装插件（`Install Plugin From Disk...`）
- 插件会提示安装成功。

## 0x2. 如何使用

- 一般来说，在IDE窗口切出去或切回来时（窗口失去/得到焦点）会触发事件，检测是否长时间（`25`天）没有重置，给通知让你选择。（初次安装因为无法获取上次重置时间，会直接给予提示）

- 也可以手动唤出插件的主界面：

  - 如果IDE没有打开项目，在`Welcome`界面点击菜单：`Get Help` -> `Eval Reset`
  - 如果IDE打开了项目，点击菜单：`Help` -> `Eval Reset`

- 唤出的插件主界面中包含了一些显示信息，

  ```
  2
  ```

  个按钮，

  ```
  1
  ```

  个勾选项：

  - 按钮：`Reload` 用来刷新界面上的显示信息。
  - 按钮：`Reset` 点击会询问是否重置试用信息并**重启IDE**。选择`Yes`则执行重置操作并**重启IDE生效**，选择`No`则什么也不做。（此为手动重置方式）
  - 勾选项：`Auto reset before per restart` 如果勾选了，则自勾选后**每次重启/退出IDE时会自动重置试用信息**，你无需做额外的事情。（此为自动重置方式）

## 0x3. 如何更新

### 1). 插件更新机制（推荐）：

- IDE会自行检测其自身和所安装插件的更新并给予提示。如果本插件有更新，你会收到提示看到更新日志，自行选择是否更新。
- 点击IDE的`Check for Updates...` 菜单手动检测IDE和所安装插件的更新。如果本插件有更新，你会收到提示看到更新日志，自行选择是否更新。
- 插件更新可能会需要**重启IDE**。

### 2). 手动更新：

- 从本页面下载最新的插件`zip`包安装更新。参考本文：`下载安装`小节。
- 插件更新需要**重启IDE**。

## 0x4. 一些说明

- 本插件默认不会显示其主界面，如果你需要，参考本文：`如何使用`小节。

- 市场付费插件的试用信息也会**一并重置**。

- ```
  MyBatisCodeHelperPro
  ```

  插件有

  两个

  版本如下，功能完全相同，安装时须看清楚！

  - [MyBatisCodeHelperPro (Marketplace Edition)](https://plugins.jetbrains.com/plugin/14522-mybatiscodehelperpro-marketplace-edition-)，可重置！
  - ~~[MyBatisCodeHelperPro](https://plugins.jetbrains.com/plugin/9837-mybatiscodehelperpro)，不可重置！~~

- 对于某些付费插件（如:

   

  ```
  Iedis 2
  ```

  ,

   

  ```
  MinBatis
  ```

  ）来说，你可能需要去取掉

  ```
  javaagent
  ```

  配置（如果有）后重启IDE：

  - 如果IDE没有打开项目，在`Welcome`界面点击菜单：`Configure` -> `Edit Custom VM Options...` -> 移除 `-javaagent:` 开头的行。
  - 如果IDE打开了项目，点击菜单：`Help` -> `Edit Custom VM Options...` -> 移除 `-javaagent:` 开头的行。

- 重置需要**重启IDE生效**！

- 重置后并不弹出`Licenses`对话框让你选择输入License或试用，这和之前的重置脚本/插件不同（省去这烦人的一步）。

- 如果长达`25`天不曾有任何重置动作，IDE会有**通知询问**你是否进行重置。

- 如果勾选：`Auto reset before per restart` ，重置是静默无感知的。

- 简单来说：勾选了`Auto reset before per restart`则无需再管，一劳永逸。

## 0x5. 开源信息

- 插件是学习研究项目，源代码是开放的。源码仓库地址：[Gitee](https://gitee.com/pengzhile/ide-eval-resetter)。
- 如果你有更好的想法，欢迎给我提`Pull Request`来共同研究完善。
- 插件源码使用：`GPL-2.0`开源协议发布。
- 插件使用`PHP`编写，毕竟`PHP`是世界上最好的编程语言！
