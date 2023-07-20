---
title: 如何使用github/hexo发布博客
---

## date: 2023-03-12 16:33:26

tags:执行hexo n "如何使用github/hexo发布博客" ，就会在blog/source/_posts路径下生成一个md文件，用markdown工具编辑好之后，御三套（
**hexo clean**
**hexo g** 
**hexo d**）走一波就可以你写的内容啦

使用“---

标题

---”命名标题

# C++函数分文件编写配置

- mingw32-make打mingw后按tab自动补全
- mingw32-make clean会清理掉.o和.exe文件
- output/main

# Hexo插入图片

解决方案如下：

修改配置文件
在根目录下配置文件_config.yml 中有 post_asset_folder:false改为true。
这样在建立文件时，Hexo会自动建立一个与文章同名的文件夹，这样就可以把与该文章相关的所有资源（图片）都放到那个文件夹里方便后面引用
我这里放的图片是：sky2.jpg
安装插件：hexo-asset-image(插件经测试无问题)

```
npm install https://github.com/7ym0n/hexo-asset-image --save（使用cnpm速度相当会快点，当然npm也可以滴）
使用这个插件来引入图片，而不是网上那些方法里说的用传统md语法相对路径的方法。
插入图片时用这种方式：
{% asset_img sky2.jpg This is an test image %}
其中sky2.jpg就是你要引用的图片，我这里就是sky2.jpg，后面的This is an test image是图片描述，可以自己修改。
```
