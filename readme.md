## 基本结构

有两个分支，一个是hexo，默认分支，存着的是安装hexo需要的文件，便于换机的时候使用，只要将source的文件替换成你当前的source就可以了，然后执行npm install，就会根据package.json安装依赖，当然是已经装好node和git的前提下。


第二个分支main，是page会使用到的分支。在hexo的config文件里面和github page设置里面配好就可以了

## 新机安装流程：：

```
git init
## 默认会拉hexo，因为hexo是默认分支
git clone git@github.com:yima-hmh/yima-hmh.github.io.git
## 本地创建hexo分支，要有一样的分支才能push，否则会报错
git checkout -b hexo 
```
## 遇到的问题



Q：新建了hexo分支后，如何将需要的文件传上去，也就是如何进行初始化？

A：
git 要先拉取整个仓库，或者说至少了解整个仓库的结构才能做push


初始化hexo我做了什么：
```
git clone git@github.com:yima-hmh/yima-hmh.github.io.git

git pull

git checkout -b hexo #本地创建新分支并切换到该分支
```
然后将所需要的文件放到这个文件夹中，然后执行
```
git config user.name 'yima'
git config user.email '1716645823@qq.com'
git add .
git commit -m ''
git push -f origin hexo ##即使hexo是默认的，也还是要指定分支的
```

另外记得添加readme文件是要大写的啊，才会显示

## 也许有用的git知识
```
git remote add  origin git@github.com:yima-hmh/yima-hmh.github.io.git

```