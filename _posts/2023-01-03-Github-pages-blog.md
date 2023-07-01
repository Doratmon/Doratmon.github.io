---
layout: post
title: Github pages+Jekyll搭建个人博客
author: leanwe
tags:
- 软技能
date: 2023-01-03 21:53 +0800
toc: true
---
之前一直使用notion做学习碎片记录，但内容比较零散，不够系统。最近决定notion里的碎片记录分类汇总，输出完整的文章，因此基于Github pages和Jekyll搭建了个人博客。也有考虑过基于wordpress和阿里云服务器来搭建，但这样需要自己购买域名，而且Jekyll搭建博客已经比较成熟，模版美观，因而最终选择了该方案。
# 1 Github Pages和Jekyll简介
开发者可以用Github Pages来创建项目介绍的静态网站，其本质上是利用了Github的仓库。这里我们用Github Pages来搭建个人博客。

Jekylly是一个静态网站生成器,将纯文本转化为静态网站和博客；由于GitHub Pages生成的是静态页面，每次更新博客都需要手动更改HTML，这就使得每次写博客都变得很麻烦，而用了这个工具以后，它会根据预先设置好的格式来生成博客内容，我们就无需关心html代码，只需要把重心放在博客的写作上。
# 2 步骤
## 2.1 创建Github仓库
仓库名格式为`username.github.io`，且需要设为Public，不然不能通过域名访问。
![博客仓库名](/assets/imgs/博客仓库名.png)
## 2.2 安装Jekyll
安装Jekyll并不是必选项，这是为了可以在本地预览博客效果。我是mac环境，因此采用homebrew安装。这里换成了国内中科大的源，安装homebrew的指令如下
```bash
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
安装chruby、ruby-install和xz，chruby是ruby的版本管理工具
```bash
brew install chruby ruby-install xz
```
用ruby-install安装ruby-2.7.7，因为jekyll的许多theme还未兼容ruby-3.x（比如我选择的Not Pure Poole），所以这里选择了较低的版本。
```bash
ruby-install ruby 2.7.7
```
修改shell配置文件.zshrc，如果shell默认是bash则修改.bash_profile
```bash
echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
echo "chruby ruby-2.7.7" >> ~/.zshrc # run 'chruby' to see actual version
```
安装jekyll和bundler gems，gem是可以包含在ruby项目里的代码。
```bash
gem install jekyll bundler
```
## 2.3 选择博客主题
在[jekyll theme](http://jekyllthemes.org/)中包含了许多主题，预览后我选择的是`Not Pure Poole`，用`git clone`下载到本地。
```bash
git clone https://github.com/vszhub/not-pure-poole
mv not-pure-poole MyBlog # 重命名目录为MyBlog
```
## 2.4 关联本地仓库与远程仓库
克隆下来的仓库并未与Doratmon.github.io仓库关联，所以这里要建立关键，后面才能进行push、pull等操作。
```bash
git remote -v # 查看关联的远程仓库
    origin https://github.com/vszhub/not-pure-poole.git (fetch) 
    origin https://github.com/vszhub/not-pure-poole.git (push)

git remote remove origin # 移除已有关联
git remote add origin git@github.com:Doratmon/Doratmon.github.io.git # 建立新的关联
```
## 2.5 在本地测试jekyll
```bash
cd MyBlog
bundle install
bundle exec jekyll serve
```
出现以下内容后在浏览器输入`http://127.0.0.1:4000`访问。
![jekyll本地测试](/assets/imgs/jekyll%E6%9C%AC%E5%9C%B0%E6%B5%8B%E8%AF%95.png)
正常访问就会出现和预览效果一致的博客首页。
![博客首页](/assets/imgs/%E5%8D%9A%E5%AE%A2%E9%A6%96%E9%A1%B5.png)
## 2.6 修改本地分支名并提交到远程仓库
由于[弗洛伊德事件](https://baike.baidu.com/item/%E4%B9%94%E6%B2%BB%C2%B7%E5%BC%97%E6%B4%9B%E4%BC%8A%E5%BE%B7/50241399?fr=aladdin)，Github的默认分支名已经从master改为了main，但本地默认分支名还是master（Not Pure Poole主题是这样的，可能是git的原因，并未深究），所以需要将本地默认分支名改为main，否则push时会出错。
```bash
git branch -m master main
git add .
git commit -m "init my blog"
git push --set-upstream origin main # 第一次提交需要用此命令，以后就可以用git push了。
```
待Github仓库检查通过后，就可以通过域名https://Doratmon.github.io访问了。
# 3 个性化博客
_config.yml解析、博客项目布局介绍。。。


[b站教程](https://www.bilibili.com/video/BV1C14y187Nh/?spm_id_from=333.999.0.0&vd_source=764f24a66f60b983c5efdbc30c60d922)

[jekyll官网](https://jekyllrb.com/)


