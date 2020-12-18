####手把手教你不花一分钱搭建个人博客网站
本片文章会详细介绍使用GitHub+GitBook方式搭建属于自己的个人知识管理博客。
* **真**不花一分钱：依托GitBook平台同步GitHub搭建个人博客
* 私人博库：在自己的服务器上搭建专属博客。

#### GitBook是什么
  在说GitBook不得先说GitHub。GitHub相比大家都不陌生，GitHub是现在世界上主流的开源项目源码云托管平台。可以非常方便的管理团队协同管理不同版本，不同开发人员的不同需求。
  而GitHub不仅是程序员们管理代码的工具，它可以应该用在各种需要多人员协作的场景中。
现在我们说回GitBook，虽然都是**”Git“**开头，但GitBook主要是基于 Markdown 和 AsciiDoc 两种文档语法，

#### 真不花一分钱：依托GitBook平台同步GitHub搭建个人博客
##### GitHub注册地址如下 [点击跳转](https://github.com/join?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home)
![1](../image/98/1.jpg)
##### GitBook官往地址如下[点击跳转](https://app.gitbook.com/join)
![2](../image/98/2.jpg)
##### 登录GItBook创建一个新空间
![3](../image/98/3.jpg)
##### 创建公开/私人项目
![4](../image/98/4.jpg)
##### 点击创建进入初始界面
![5](../image/98/5.jpg)
##### 进入GitHub新建项目[点击进入创建页面](https://github.com/new)
![6](../image/98/6.jpg)
![6-1](../image/98/6-1.jpg)
#####下载Git图形界面管理工具SourceTree [点击跳转官方网站下载](https://www.sourcetreeapp.com/)
![7](../image/98/7.jpg)
#####打开SouceTree
![8](../image/98/8.jpg)
#####使用SourceTree添加GitHub账号或OAuth协议授权
![8-1](../image/98/8-1.jpg)
![8-2](../image/98/8-2.jpg)
![8-3](../image/98/8-3.jpg)
![8-4](../image/98/8-4.jpg)
#####GitHub HTTPS协议克隆项目
![8-5](../image/98/8-5.jpg)
![8-6](../image/98/8-6.jpg)

####下面就是重头戏了
##### 在gitbook_test目录下创建如下
![9](../image/98/9.jpg)
```shell
### GitBook默认加载文件
README.md      
### GitBook默认加载文件 如果不配置.gitbook.yaml 默认使用
SUMMARY.md     

### git不提交list
cat .gitignore  
   .DS_Store   --文件名为.DS_Store 隐藏文件不会提交到GitHub中
   

```

##### 自定义配置GitBook目录结构（修改 .gitbook.yaml）
```shell
### GitBook 默认目录
root: ./Content 

#structure:
#    readme: README.md
#    summary: SUMMARY.md

#redirects:
#    previous/page: new-folder/page.md
```