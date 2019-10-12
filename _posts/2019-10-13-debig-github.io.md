---
layout: post
title: debig github.io jekyll
date: 2019-10-13
tags: github jekyll ruby
---

大概就是 github 沒有正常工作應該是有 md 寫錯了 要 debug 來電腦端看 比較快

這邊 debig echochio-tw.github.io ... 您可換您所需

```
git clone https://github.com/echochio-tw/echochio-tw.github.io.git
cd echochio-tw.github.io
apt -y install ruby ruby-dev
gem install bundler
gem install jekyll
bundle install
bundle exec jekyll serve
```

 我的有少 Gemfile .....

Gemfile
```
source "https://rubygems.org"
gem "minima"
group :jekyll_plugins do
	   gem "jekyll-paginate"
end
```

放回去 ....
```
rm -rf .git
git init
git add .
git commit -m "updated site"
git remote add origin https://github.com/echochio-tw/echochio-tw.github.io.git
git push -u origin master
```
