# web_robot
自动化网页操作机器人

## 详细说明
请见博客   
[使用教程V1.0版本](http://ganjiacheng.cn/article/article_18_chrome%E6%8F%92%E4%BB%B6-%E7%BD%91%E9%A1%B5%E8%87%AA%E5%8A%A8%E5%8C%96/)  
[持续更新教程](http://ganjiacheng.cn/article/article_21_chrome%E6%8F%92%E4%BB%B6-WEB-ROBOT/)


## 已有功能
1. 管理多个事务，每个事务有多个事件，每个事件对应一种操作   
2. 新增事件中方便的页面元素筛选器，querySelect自由筛选器
3. 可以测试运行一个事件，运行一整个事务。
4. 支持事务的导入导出
5. 支持源码事务，写js源码并注入运行
6. 支持流程事务的受控运行，本地鼠标和键盘还原事件。
7. 支持受控事务，实现键鼠录制和还原
8. 支持元素筛选和执行时的自动定位
9. 支持设值事件作为运行前自定义参数${value}
10. 支持页面直接添加事件
11. 支持定时运行
12. 支持源码事务的开启直接注入


## 核心部分--事务和运行机制说明

新建事务分为三种事务，流程事务，源码事务，受控事务；  
运行分为运行，定时运行，受控运行，轮播，开启注入；

- 流程事务：通过dom定义事件。  
    - 运行 (运行一次，运行在浏览器后台，使用浏览器事件)
    - 受控运行 (运行一次，运行在本地客户端，控制鼠标键盘还原对应事件)
    - 定时运行 (定时运行，运行在浏览器后台，有两种定时模式每日和每隔，使用浏览器事件)
    - 轮播 (循环运行事件，运行在插件页，插件页关闭即停止，使用浏览器事件)

- 源码事务：通过源码定义事件，有正则地址匹配机制。
    - 运行 (运行一次，直接向目前页注入代码)
    - 定时运行 (定时运行，运行在浏览器后台，向当前页注入固定代码)
    - 开启注入 (打开页面时注入，直接匹配地址，进行注入)

- 受控事务：通过鼠标键盘录制定义事件，可以受控运行，  
    - 受控运行 (运行一次，运行于本地客户端，还原录制的鼠标键盘事件)

流程事务事件的定义包括 dom节点，事件，延时，设值： 

- 节点筛选器包含
  - 自定义节点筛选器
  - html标签筛选器

- 事件包含
  - click 点击
  - value 设值
  - refresh 刷新
  - pagejump 当页跳转

注：受控相关的都必须使用开启本地客户端。

## 使用方法
1. 浏览器设置（三个点）--> 更多工具 --> 扩展程序 ↓  
2. 打开右上角开发者模式 --> 加载已解压的扩展程序 --> 选择clone下来的该项目根目录 ↓  
3. 弄完可关掉开发者模式 --> 右键项目图标 --> 检查可读取和更改网站数据 --> 在所有网站上

## 版本更新

> git pull

在继续上面的1，2步骤

## 受控运行，需开启本地客户端web服务

1. 首先准备一个python3虚拟环境，venv/ 放于项目根目录下，如有自己的python3，请修改py/web.py中的PYTHON_ENV
- PYTHON_ENV = "./venv/bin/python"
2. pip下载 py/requirements.txt 里的包
3. 项目根目录下启动web服务 **python py/web.py**
4. 如果没反应，以mac举例，左上角的设置 -> 系统偏好设置 -> 安全性与隐私 -> 辅助功能 将开启web服务的应用(如iTerm)加入到里面

## 使用场景

- 流程事务可以定义复杂重复的页面操作进行自动化，直接运行适用比如重复填写负责的表单，设值也可以运行时自定义参数。
- 流程事务的定时运行可以适用每日签到，每日在网页处理某件同样的事。
- 流程事务受控运行适用于前端做了特殊处理无法触发事件的情况，使用键盘鼠标模拟事件，必然可以触发。
- 源码事务的的定时运行适用于如定时提醒喝水(alert)等
- 源码事务的开始注入适用于如百度去广告的场景等
- 受控事务的录制和受控运行适用于对一个复杂操作(无法用流程实现)的定义和复现。

## 版本迭代

v0.1 (2019.08.14)
1. 完成初始第一版，管理流程事务

v1.0 (2020.05.13)(重构更新)
1. 管理多个事务，每个事务有多个过程，每个过程对应一种操作   
2. 新增操作中方便的页面元素筛选器，css/id筛选器
3. 测试运行一个过程，运行一个事务，运行转为background后台   
4. 支持事务的导入导出

v1.1 (2020.05.28) (数据与上版本不兼容)
1. 支持源码事务

v1.2 (2020.06.01)
1. 新增事务受控运行模式，运行于background中
2. 新增本地web服务，用于鼠标键盘模拟流程的受控执行

v1.2.1 (2020.06.03)
1. 删除事务新增校验
2. 实现流程中事件的复制，移动，编辑

v1.3 (2020.06.05)
1. 新增受控事务
2. 本地客户端实现受控事务的键鼠事件录制，存储和还原

v1.3.1 (2020.06.06)
1. 优化元素筛选器进行自动定位
2. 优化流程事件运行和受控运行的自动定位

v1.4.0 (2020.06.07)
1. 改class/id筛选器为自由筛选器
2. 使用promise重构流程执行
3. 新增执行前自定义参数
4. 新增本地服务检查

v1.5.0 (2020.06.08)
1. 新增网页直接添加流程事件的方式

v1.6.0 (2020.06.09)
1. 新增定时运行, 仅支持流程事务

v1.6.1 (2020.06.10)
1. 修改源码事务，可进行路由匹配检查
2. 新增源码事务的定时运行

v1.6.2 (2020.06.13)
1. 优化运行元素的定位
2. 新增源码事务直接注入的开启与关闭

# 项目规划

目前项目为1.6.2版本，已实现功能如上

**目前项目可能会有的两个分支**

- 自动化执行流程的优化
- 专为自动化测试的插件

两个分支并不冲突，可以一起做，上面的自动化执行流程为自动化测试插件中的一部分。

## 专为自动化测试的插件

- 会有一个项目管理的概念，增删改查，运行全部用例
- 运行完用例需要有运行报告
- 一个项目里包含多个用例，可调试，增，删，改每个用例
- 一个用例包含用例输入参数，自动化执行流程的事件，校验事件
- 执行用例自定义参数是一个列表，每一项是一个{}，kv存储，参数可以是流程事件用到，也可以是校验事件用到，执行时会按参数列表批量执行这个用例。
- 自动化流程的事件为使用上面的事件流程
- 校验事件需要一个全新的事件定义，包括单元素校验，列表校验，源码校验等。
- 自动化测试必须采用受控运行，需要优化这个模式
- 自动化测试的筛选器选用id/class/高级筛选器

## 其他优化

- 优化界面设计
- 优化部分代码逻辑

## 感谢轮子
1. [materializecss](http://www.materializecss.cn/about.html)
3. [官方轮子](https://developer.chrome.com/extensions)
4. [插件教程](https://www.cnblogs.com/liuxianan/p/chrome-plugin-develop.html)


## 感谢Contributors，欢迎加入

- [webgjc](https://github.com/webgjc)
- [ILovePing](https://github.com/ILovePing)

### License

web_robot is [MIT licensed](./LICENSE).