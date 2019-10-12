---
layout: post
title: Mojolicious helloword
date: 2018-11-03
tags: Mojolicious perl
---
Mojolicious安装：

在 windows 安裝測試機 ...用 vagrant

附上 vagrantfile

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/centos-7.5"
  # If you run into issues with Ansible complaining about executable permissions,
  # comment the following statement and uncomment the next one.
  config.vm.synced_folder ".", "/vagrant"
  # config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end
  config.vm.define :user, primary: true do |user|
    user.vm.network :forwarded_port, host: 8080, guest: 8080
	user.vm.network :forwarded_port, host: 3000, guest: 3000
    #user.vm.network :forwarded_port, host: 5000, guest: 5000
    #user.vm.network :forwarded_port, host: 9999, guest: 9999
    user.vm.network :forwarded_port, host: 2201, guest: 22, id: "ssh", auto_correct: true
    user.vm.network "private_network", ip: "192.168.50.91"
    # added dynamic swap file management to prevent "Free Swap Space: 0 B" warning in Jenkins
    #user.vm.provision "shell", path: "bootstrap.sh"
    #user.vm.provision :shell, inline: 'ansible-playbook /vagrant/ansible/user.yml -c local -v'
    #user.vm.hostname = "cd"
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
# if Vagrant.has_plugin?("vagrant-proxyconf")
#   config.proxy.http     = "http://proxy.company.com:8080/"
#   config.proxy.https    = "http://proxy.company.com:8080/"
#   config.proxy.no_proxy = "localhost,127.0.0.1"
# end
end
```



裝Centos ...及更新還有 update , 

```
yum install -y epel-release
yum update
yum groupinstall -y "Development Tools"
```

不知為何 OS 內的 perl 裝 Mojolicious 怪怪的 .... 好吧 ...自己來裝 perl 

```
wget https://www.cpan.org/src/5.0/perl-5.28.0.tar.gz
tar zxvf perl-5.28.0.tar.gz
cd perl-5.28.0
./Configure -des -Dprefix=/usr
make ; make install
```

看 perl 

```
# perl -v

This is perl 5, version 28, subversion 0 (v5.28.0) built for x86_64-linux

Copyright 1987-2018, Larry Wall

Perl may be copied only under the terms of either the Artistic License or the
GNU General Public License, which may be found in the Perl 5 source kit.

Complete documentation for Perl, including FAQ lists, should be found on
this system using "man perl" or "perldoc perl".  If you have access to the
Internet, point your browser at http://www.perl.org/, the Perl Home Page.

```

用 CPAN 來裝 ....啥 ...不知 CAPN 那不要玩 perl 了 ....XD

```
perl -MCPAN -e shell
install Mojolicious
```

寫個  helloword

```
#! /usr/bin/perl
use Mojolicious::Lite;

get '/:foo' => sub {
  my $c = shift;
  my $foo  = $c->param('foo');
  $c->render(text => "Hello from $foo.");
};

app->start;
```

就執行看看 .....

```
# chmod +x hello.pl
#  ./hello.pl  daemon -m production -l http://*:8080
[Sat Nov  3 03:17:41 2018] [info] Listening at "http://*:8080"
Server available at http://127.0.0.1:8080
```

到本機端 ....

```
# curl http://127.0.0.1:8080/aaaa
Hello from aaaa
#
```

當然你不想放前端 ... 那用 nginx 當 proxy ...

```
upstream myapp {
  server 127.0.0.1:3000;
}
server {
  listen 8080;
  server_name localhost;
  location / {
    proxy_pass http://myapp;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```
