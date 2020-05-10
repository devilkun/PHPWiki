## 声明: 此方法适用于phpstorm2019.3版本

## 使⽤⽅法

1. 先下载压缩包解压后得到jetbrains-agent.jar，把它放到你认为合适的⽂件夹内。
蓝奏云: https://lanzous.com/icg379c

下载⻚⾯：https://zhile.io/2018/08/17/jetbrains-license-server-crack.html


2. 启动你的IDE，如果上来就需要注册，选择：试⽤（Evaluate for free）进⼊IDE。

3. 点击你要注册的IDE菜单： Configure 或 Help -> **Edit Custom VM Options ...**

如果提示是否要创建⽂件，请点 Yes 。

参考⽂章：https://intellij-support.jetbrains.com/hc/en-us/articles/206544869

4. 在打开的vmoptions编辑窗⼝末⾏添

加： -javaagent:/absolute/path/to/jetbrains-agent.jar

⼀定要⾃⼰确认好路径(不要使⽤中⽂路径)，填错会导致IDE打不开！！！最好使⽤绝对路径。

⼀个vmoptions内只能有⼀个 -javaagent 参数。

如果你是⽆外⽹环境，在javaagent路径后加 =offline ，

如： -javaagent:/absolute/path/to/jetbrains-agent.jar=offline

 示例
linux: -javaagent:/home/neo/jetbrains-agent.jar

windows: -javaagent:D:\soft\work\jetbrains-agent\jetbrains-agent.jar

如果还是填错了，参考这篇⽂章编辑vmoptions补救：

https://intellij-support.jetbrains.com/hc/en-us/articles/206544519

5. 重启你的IDE。

6. 点击IDE菜单 Help -> Register... 或 Configure -> Manage License...

⽀持两种注册⽅式：**License server** 和 **Activation code**:

1. 选择License server⽅式，地址填⼊： http://fls.jetbrains-agent.com （⽹络不佳的

⽤第2种⽅式）

2. 选择Activation code⽅式离线激活，请使⽤： ACTIVATION_CODE.txt 内的注册码激活。

如果激活窗⼝⼀直弹出（**error 1653219**），请去**hosts**⽂件⾥ 移除 **jetbrains**相关的项⽬。

**License key is in legacy format == Key invalid**，表示**agent**配置未⽣效。如果你需要⾃定义License name，请访问：https://zhile.io/custom-license.html

本项⽬在最新2019.3.3上测试通过。

