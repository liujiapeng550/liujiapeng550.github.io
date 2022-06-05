---
title: 如何搭建githpage个人blog
date: 2022-05-24 11:27:25
tags: blog github hexo
typora-root-url: ..
top_img: images/如何搭建githpage个人blog/top_img.jpg
cover: images/如何搭建githpage个人blog/top_img.jpg
categories: Hexo
---

# 1.hexo博客基础环境搭建（非常详细）

[hexo博客基础环境搭建](https://unclevicky.github.io/rabbitBear/2021/02/07/hexo%E5%8D%9A%E5%AE%A2%E5%9F%BA%E7%A1%80%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/)

## 1.**安装git**

## 2. **安装nodejs**

## 3. **安装hexo**  

   1. 在nodejs环境中（cmd或者bash）安装hexo包

      ```
      npm install -g hexo-cli
      ```
      
   2. 测试hexo是否成功

      - 初始化

        ```
        hexo init myBlog
        ```
        
      - 安装基础环境包
      
        ```
        cd myBlog
        npm install
        ```
        
        执行成功后，myBlog文件夹的结构如下:
        
        
      
      ```
      BASH
      myBlog
      ├── _config.yml # 网站的配置信息，您可以在此配置大部分的参数。 
      ├── package.json
      ├── scaffolds # 模版文件夹
      ├── source  # 资源文件夹，除 _posts 文件，其他以下划线_开头的文件或者文件夹不会被编译打包到public文件夹
      |   ├── _drafts # 草稿文件
      |   └── _posts # 文章Markdowm文件 
      └── themes  # 主题文件夹
      ```
      
      - 本地启动hexo服务器进行测试
      
        ```
        hexo s
        ```
        
            在浏览器中输入
        
        [http://localhost:4000](http://localhost:4000/)
        
        ，如果看到如下效果，则证明hexo环境搭建成功:
        
        ![image-20220524114542277](https://mypicgo-1256286372.cos.ap-guangzhou.myqcloud.com/images/image-20220524114542277.png)
        

# 2.Hexo+Typora写文章时图片显示不一致

[Hexo+Typora写文章时图片显示问题]: https://juejin.cn/post/6998539442881298469

使用会发现Hexo和typora 图片路径显示不一致，不能同时正常显示。

1.. Typora中图片路径

![image-20220524113638326](https://mypicgo-1256286372.cos.ap-guangzhou.myqcloud.com/images/image-20220524113638326.png)

../images/${filename}，文件保存到(自己的博客路径)/source/images

2. 设置文章根目录

   ![a611e7350b7b4838bea7e362f3796c0ctplv-k3u1fbpfcp-zoom-in-crop-mark1304000](/images/如何搭建githpage个人blog/a611e7350b7b4838bea7e362f3796c0ctplv-k3u1fbpfcp-zoom-in-crop-mark1304000.webp)

路径设置为source

# 3 发布

在本地执行hexo的编译和发布命令，则可成功发布到github上

```
hexo clean # 删除以前的发布
hexo g -d  # 重新编译生成，并按配置文件中的github信息，发布到对应的网站上
```
