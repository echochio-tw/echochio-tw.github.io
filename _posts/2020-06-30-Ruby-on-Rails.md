---
layout: post
title: ROR install
date: 2020-06-30
tags:  Ruby on Rails
---

Centos7 安裝 ROR ... 還真坑 ....


```
yum clean all
yum -y update
```

```
yum install -y git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel
```

install rbenv
```
cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
```

vm .bash_profile
```
export PATH=/home/chio/.rbenv/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"
```

source ...
```
source ~/.bash_profile
```

install ruby-build
```
git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
```

vm .bash_profile
```
export PATH=/home/chio/.rbenv/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"
```

source ....
```
source ~/.bash_profile
```

rbenv install -l
```
2.5.8
2.6.6
2.7.1
jruby-9.2.11.1
maglev-1.0.0
mruby-2.1.1
rbx-5.0
truffleruby-20.1.0

```

install ruby
```
rbenv install -v 2.5.8
rbenv global 2.5.8
ruby -v
```

centos7 install sqlite > 3.8 & yarn
```
wget https://www.sqlite.org/2019/sqlite-autoconf-3290000.tar.gz
tar xzvf sqlite-autoconf-3290000.tar.gz
cd sqlite-autoconf-3290000
./configure --prefix=/opt/sqlite/sqlite3
make 
make install
/opt/sqlite/sqlite3/bin/sqlite3 --version
curl --silent --location https://rpm.nodesource.com/setup_12.x | sudo bash -
yum install nodejs
rpm --import https://dl.yarnpkg.com/rpm/pubkey.gpg
wget https://dl.yarnpkg.com/rpm/yarn.repo -O /etc/yum.repos.d/yarn.repo
yum install yarn
```

gem sqlite3
```
gem install sqlite3 -- --with-sqlite3-include=/opt/sqlite/sqlite3/include --with-sqlite3-lib=/opt/sqlite/sqlite3/lib
```

gem bundler
```
gem install bundler
```

gem rails
```
gem install rails
rbenv rehash
rails -v
```

run .....
```
cd ~
rails new testapp
cd testapp
rake db:create
rails server --binding=Your-Server-IP
```
