#### 手把手教你不花一分钱搭建个人博客网站
本文章会详细介绍使用GitHub+GitBook方式搭建属于自己的个人知识管理博客。**真**不花一分钱：依托GitBook平台同步GitHub搭建个人博客



#### GitBook是什么
  在说GitBook不得先说GitHub。GitHub大家都不陌生，GitHub是现在世界上主流的开源项目源码云托管平台。可以非常方便的管理团队协同管理不同版本，不同开发人员的不同需求。
  而GitHub不仅是程序员们管理代码的工具，它可以应该用在各种需要多人员协作的场景中。
现在我们说回GitBook，虽然都是**”Git“**开头，但GitBook主要是基于 Markdown 和 AsciiDoc 两种文档语法，在线展示网站。

#### 真不花一分钱：GitBook+GitHub搭建个人博客
##### GitHub注册地址如下 [点击跳转](https://github.com/join?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home)
![1](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/1.jpg)
##### GitBook官网地址如下[点击跳转](https://app.gitbook.com/join)
![2](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/2.jpg)
##### 登录GItBook创建一个新空间
![3](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/3.jpg)
##### 创建公开/私人项目
![4](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/4.jpg)
##### 点击创建进入初始界面
![5](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/5.jpg)
##### 进入GitHub新建项目[点击进入创建页面](https://github.com/new)
![6](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/6.jpg)
![6-1](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/6-1.jpg)

##### 下载Git图形界面管理工具SourceTree [点击跳转官方网站下载](https://www.sourcetreeapp.com/)
![7](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/7.jpg)
##### 打开SouceTree
![8](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/8.jpg)
##### 使用SourceTree添加GitHub账号或OAuth协议授权
![8-1](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/8-1.jpg)
![8-2](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/8-2.jpg)
![8-3](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/8-3.jpg)
![8-4](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/8-4.jpg)

##### GitHub HTTPS协议克隆项目
![8-5](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/8-5.jpg)
![8-6](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/8-6.jpg)

#### 下面就是重头戏了
##### 在gitbook_test目录下创建如下
![9](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/9.jpg)
```shell
### GitBook默认加载文件
README.md      
### GitBook默认加载文件 如果不配置.gitbook.yaml 默认使用
SUMMARY.md     


##下面属于自定义环节
### Content目录
Content
### image目录存储文章图片
image

### .gitignore 管理不提交文件
cat .gitignore  
   .DS_Store   --文件名为.DS_Store 隐藏文件不会提交到GitHub中
   

```

##### 自定义配置GitBook目录结构（修改 .gitbook.yaml）
```shell

#用于定位内容的根目录
#默认值是存储库的根目录。
root: ./
 
# Files to use as SUMMARY/README.
#（相对于<root>目录）
structure:
  # 描述：使用 README.md文件内容
  readme: README.md
  # 概要：使用 SUMMARY.md文件内容
  summary: SUMMARY.md
 
#将URL重定向到特定文件（相对于<root>目录）
redirects: new-folder/page.md
```
##### SUMMARY.md

```markdown
##大数据
* [初认Hadoop](Content/chapter3.md)

## 工具
* [GitBook安装教程](Content/chapter98.md)
* [Amberi安装HadoopJ教程](Content/chapter99.md)

```
##### SUMMARY.md 效果如下
![10](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/10.jpg)
#### 使用SourceTree提交修改
![11](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/11.jpg)
![12](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/12.jpg)

#### 在GitBook授权GitHub项目
![13](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/13.jpg)
![14](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/14.jpg)
![15](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/15.jpg)
![16](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/16.jpg)
![17](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/17.jpg)
![18](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/18.jpg)
![19](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/19.jpg)

#### 在分享中找到你的项目外网地址
![20](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/20.jpg)
#### 我们就可以尽情访问了
![21](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/21.jpg)

#### GitHub修改手动更新
![22](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/22.jpg)



#### 

#### 推荐大家一本好书









![](https://cdn.jsdelivr.net/gh/xiaopoyu/picture/images/qrcode_for_gh_d3387b21d6ac_258.jpg)
