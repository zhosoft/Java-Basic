> 2022.4.7

# VScode

- **Vetur**：VScode官方钦定Vue插件，Vue开发者必备。内含语法高亮，智能提示，emmet，错误提示，格式化，自动补全，debugger等实用功能；

- **REST Client** ：这款神器可以让我们在vscode里面进行接口调试，提供丰富的api配置方式，让我们不用离开编辑器也可以随时调用接口调试，新建一个.http文件，写下基本的测试代码，点击 Send Request即可在右边窗口查看接口返回结果，非常nice，强烈推荐；

- **Live Server**

  修改默认浏览器：https://my.oschina.net/u/4002348/blog/3154559

# Vue安装





## 安装遇见的问题

### vue ui 通过浏览器进行新建项目，最后一步出现如图错误提示：

![image-20220428203246533](..\note\Vue2.0&Vue3.0笔记.assets\image-20220428203246533.png)

卸载掉nodejs的及缓存目录（通过命令npm uninstall vue-cli -g 删除失败）

1、重新安装nodejs，并重新设置一个安装目录；例如：之前安装的是“D:\Program Files\nodejs”，现在“D:\Programs\nodejs”

2、npm config list 查看一下npm 的配置信息,下图

![image-20220428203602378](..\note\Vue2.0&Vue3.0笔记.assets\image-20220428203602378.png)

然后找到这个红色线里面的路径，看看有没有vue.cmd的文件，如果没有添加到环境变量中：

NODE_PATH：D:\Programs\nodejs

PATH:%NPM%

![image-20220428203814443](..\note\Vue2.0&Vue3.0笔记.assets\image-20220428203814443.png)

![image-20220428203846949](..\note\Vue2.0&Vue3.0笔记.assets\image-20220428203846949.png)

### 提示无法加载vue.psl

![微信截图_20220428123912](..\note\Vue2.0&Vue3.0笔记.assets\微信截图_20220428123912.png)

# Vue简介





笔记：https://www.yuque.com/cessstudy/kak11d/smaqmn

特点：

- 采用组件化模式，提供代码复用率，且让代码更好维护；
- 声明式编码，让编码人员无需直接操作DOM，提高开发效率；

