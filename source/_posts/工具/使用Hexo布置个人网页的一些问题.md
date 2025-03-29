---
title: 使用Hexo布置个人网页的一些问题
description: '使用Hexo布置个人网页的一些问题,包括如何上传本地图片hexo'
tags: 工具部署
categories:
  - 工具部署
typora-root-url: 使用Hexo布置个人网页的一些问题
abbrlink: d69f
date: 2023-07-01 20:14:06
---

### 插入图片的问题

![image-20230701202125916](image-20230701202125916.png)

- 配置URL

  ![image-20230701202211848](image-20230701202211848.png)

- 当初那个图片工具的配置就应该一动不动。

- 我想构造的是一个分类文件夹里面放相同的文件，然后图片存进去img目录中，以文件名建立一个文件夹存。像这样

  ![image-20230710174450967](/image-20230710174450967.png)

  #### 步骤如下

  1. 修改markdown内的图片设置,设置如下![image-20230710174721892](/image-20230710174721892.png)
  2. 此时插进去图片都将变成相对路径只有文件名的样子,需要添加一个typora-root-url,去修改骨架文件夹下的(scaffolds)_post模板文件![image-20230710174815323](/image-20230710174815323.png)
  3. 修改为如下![image-20230710175000041](/image-20230710175000041.png)
  4. 然后就在typora上可见,部署到网页中也可见啦。没想到本地的话，url不会起作用，所以-s命令看不到照片。然后我就然后这个插件好像无法使用部署耳机目录，会出错，暂时使用这种分类方式了,寻找了一天修改hexo-asset-image这个包的方式无果,就这样吧。![image-20230710232400529](/image-20230710232400529.png)

  5. 修改回配置![image-20230710232525256](/image-20230710232525256.png)
  
  6. 成功部署
  
     ![image-20230710232556693](/image-20230710232643369.png)
  
     ![image-20230710232707053](/image-20230710232707053.png)
  
     配上hexo-asset-image里的index.js的代码,就是修改这个修改了一天
  
     ```javascript
     'use strict';
     var cheerio = require('cheerio');
     
     // http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
     
     //这是个获取某个字符或者字符串片段在整个字符串种的起始位置
     //split(a,b):根据a进行字符串的划分,结果为大小为b的字符串数组,a不会出现在任何数组元素的部分而返回.
     //join(m):将字符串数组使用m来连接而返回比如['a','b'].join('-')='a-b'
     function getPosition(str, m, i) {
       return str.split(m, i).join(m).length;
     }
     
     hexo.extend.filter.register('after_post_render', function(data){
       var config = hexo.config;
       if(config.post_asset_folder){
         var link = data.permalink;
         var beginPos = getPosition(link, '/', 3) + 1;
         var appendLink = '';
         // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
         // if not with index.html endpos = link.lastIndexOf('.') + 1 support hexo-abbrlink
         if(/.*\/index\.html$/.test(link)) {
           // when permalink is end with index.html, for example 2019/02/20/xxtitle/index.html
           // image in xxtitle/ will go to xxtitle/index/
           appendLink = 'index/';
           var endPos = link.lastIndexOf('/');
         }
         else {
           var endPos = link.length-1;
         }
         link = link.substring(beginPos, endPos) + '/' + appendLink;
     
         var toprocess = ['excerpt', 'more', 'content'];
         for(var i = 0; i < toprocess.length; i++){
           var key = toprocess[i];
     
           var $ = cheerio.load(data[key], {
             ignoreWhitespace: false,
             xmlMode: false,
             lowerCaseTags: false,
             decodeEntities: false
           });
     
           $('img').each(function(){
             if ($(this).attr('src')){
               // For windows style path, we replace '\' to '/'.
               var src = $(this).attr('src').replace('\\', '/');
               if(!(/http[s]*.*|\/\/.*/.test(src)
                 || /^\s+\//.test(src)
                 || /^\s*\/uploads|images\//.test(src))) {
                 // For "about" page, the first part of "src" can't be removed.
                 // In addition, to support multi-level local directory.
                 var linkArray = link.split('/').filter(function(elem){
                   return elem != '';
                 });
                 var srcArray = src.split('/').filter(function(elem){
                   return elem != '' && elem != '.';
                 });
                 if(srcArray.length > 1)
                 srcArray.shift();
                 src = srcArray.join('/');
     
                 $(this).attr('src', config.root + link + src);
                 console.info&&console.info("update link as:-->"+config.root + link + src);
               }
             }else{
               console.info&&console.info("no src attr, skipped...");
               console.info&&console.info($(this));
             }
           });
           data[key] = $.html();
         }
       }
     });
     
     ```
  
     靠北,布置个博客,上传个图片可真难,真得好好学下英语.认真看文档才行，总是想着吃别人嚼碎了的东西可不行。。。。

### 博客链接映射的问题

---

```
npm install hexo-abbrlink --save

```

config文件中修改

``` json
# 这里的permalink后面要配这个abbrlink/,而不是abbrlink.html
# 原因乃是因为要使用那个图片插件,使用html会造成一个图片路径无法正常生成

permalink: posts/:abbrlink/
abbrlink:
    alg: crc32
    rep: hex
```

多去阅读这些源码说明

而不是看一些消化过后的知识。



![image-20230718024525888](/image-20230718024525888.png)