---
layout: post
title: python+flask+selenium(firefox)+Xvfb+uwsgi+apache in CentOS7
date: 2019-09-17
tags: centos7 python flask selenium uwsgi apache CentOS7
---

```
yum -y install python36 python36-pip
yum -y install python36 python36-devel python36-pip mod_proxy_uwsgi gcc
python3.6 -m pip install --upgrade pip
python3.6 -m pip install virtualenv flask uwsgi ... (python pypi you need ...)
```

Add User & env ...
```
adduser flask
passwd flask
su - flask
mkdir app
cd app
virtualenv -p python3 env
source env/bin/activate
python3.6 -m pip install flask uwsgi uwsgi selenium xvfbwrapper Pillow ... (install you need)
```

vi ~/app/app.py
```
#!/usr/bin/env python
from flask import Flask
from flask import send_file
import time
from selenium import webdriver
from xvfbwrapper import Xvfb
from PIL import Image
app = Flask(__name__)

@app.route("/")
def root():
    import sys
    return sys.version
 
@app.route('/get_image')
def get_image():
    vdisplay = Xvfb(width=1280, height=740)
    vdisplay.start()
    driver = webdriver.Firefox()
    time.sleep(10)
    driver.get("https://www.google.com/")
    time.sleep(5)
    driver.save_screenshot('test.png')
    driver.quit()
    vdisplay.stop()
    return send_file('test.png', mimetype='image/png')

if __name__ == "__main__":
    app.run(threaded=True)
```

vi ~/app/wsgi.py
```
from app import app as application

if __name__ == "__main__":
    application.run()
```

vi ~/app/wsgi.ini
```
[uwsgi]
module = wsgi
uid = flask
master = true
processes = 5
enable-threads = true
single-interpreter = true
socket = 127.0.0.1:8099
chmod-socket = 660
vacuum = true
logto = /var/log/uwsgi/flask.log
die-on-term = true
```

in root

vi /etc/systemd/system/flaskapp.service
```
[Unit]
Description=uWSGI server for flask
After=network.target

[Service]
User=flask
Group=apache
WorkingDirectory=/home/flask/app
Environment="PATH=/home/flask/app/env/bin"
ExecStart=/home/flask/app/env/bin/uwsgi --ini wsgi.ini

[Install]
WantedBy=multi-user.target
```
set log
```
mkdir /var/log/uwsgi
touch /var/log/uwsgi/flask.log
chown -R flask:flask /var/log/uwsgi
```

start service
```
systemctl enable flaskapp.service
systemctl start flaskapp.service
systemctl status flaskapp.service
```

vi /etc/httpd/conf/httpd.conf
```
Listen 12349
LoadModule proxy_uwsgi_module modules/mod_proxy_uwsgi.so
<VirtualHost *:12349>
    ProxyPass / uwsgi://127.0.0.1:8099/
</VirtualHost>
```

check apache & start
```
apachectl configtest
systemctl enable httpd
systemctl start httpd
```

if nginx ....
```
server {
    listen 12349;
    location / {
        include     /etc/nginx/uwsgi_params;
        uwsgi_pass         127.0.0.1:8099;

        proxy_redirect     off;
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Host $server_name;
    }
}

```
check nginx config
```
nginx -t 
```

When you reconfig app.py need
```
systemctl restart flaskapp.service
```

debug app.py service ...
```
tail -f /var/log/uwsgi/flask.log
```

I use ...
```
from selenium import webdriver
#from xvfbwrapper import Xvfb
from pyvirtualdisplay import Display
display = Display(visible=0, size=(800,600))
driver=webdriver.Firefox()
```

in user flask must copy ...
```
cp /usr/bin/Xvfb /home/flask/app/env/bin/.
cp /usr/bin/firefox /home/flask/app/env/bin/.
cp /usr/bin/geckodriver /home/flask/app/env/bin/.
```

find Xvfb lib
```
ldd /usr/bin/Xvfb

        linux-vdso.so.1 =>  (0x00007ffc949f9000)
        libselinux.so.1 => /lib64/libselinux.so.1 (0x00007f2c65130000)
        libcrypto.so.10 => /lib64/libcrypto.so.10 (0x00007f2c64cce000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f2c64ac9000)
        libGL.so.1 => /lib64/libGL.so.1 (0x00007f2c6483d000)
        libpixman-1.so.0 => /lib64/libpixman-1.so.0 (0x00007f2c64594000)
        libXfont2.so.2 => /lib64/libXfont2.so.2 (0x00007f2c64363000)
        libXau.so.6 => /lib64/libXau.so.6 (0x00007f2c6415f000)
        libsystemd.so.0 => /lib64/libsystemd.so.0 (0x00007f2c63f2e000)
        libxshmfence.so.1 => /lib64/libxshmfence.so.1 (0x00007f2c63d2a000)
        libXdmcp.so.6 => /lib64/libXdmcp.so.6 (0x00007f2c63b24000)
        libpam_misc.so.0 => /lib64/libpam_misc.so.0 (0x00007f2c63920000)
        libpam.so.0 => /lib64/libpam.so.0 (0x00007f2c63710000)
        libaudit.so.1 => /lib64/libaudit.so.1 (0x00007f2c634e8000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f2c631e6000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f2c62fc9000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f2c62bfc000)
        libpcre.so.1 => /lib64/libpcre.so.1 (0x00007f2c6299a000)
        /lib64/ld-linux-x86-64.so.2 (0x00005565ddc90000)
        libz.so.1 => /lib64/libz.so.1 (0x00007f2c62783000)
        libGLX.so.0 => /lib64/libGLX.so.0 (0x00007f2c62551000)
        libX11.so.6 => /lib64/libX11.so.6 (0x00007f2c62213000)
        libXext.so.6 => /lib64/libXext.so.6 (0x00007f2c62000000)
        libGLdispatch.so.0 => /lib64/libGLdispatch.so.0 (0x00007f2c61d4a000)
        libfontenc.so.1 => /lib64/libfontenc.so.1 (0x00007f2c61b43000)
        libfreetype.so.6 => /lib64/libfreetype.so.6 (0x00007f2c61883000)
        libcap.so.2 => /lib64/libcap.so.2 (0x00007f2c6167e000)
        librt.so.1 => /lib64/librt.so.1 (0x00007f2c61476000)
        liblzma.so.5 => /lib64/liblzma.so.5 (0x00007f2c6124f000)
        liblz4.so.1 => /lib64/liblz4.so.1 (0x00007f2c6103a000)
        libgcrypt.so.11 => /lib64/libgcrypt.so.11 (0x00007f2c60db9000)
        libgpg-error.so.0 => /lib64/libgpg-error.so.0 (0x00007f2c60bb3000)
        libresolv.so.2 => /lib64/libresolv.so.2 (0x00007f2c6099a000)
        libdw.so.1 => /lib64/libdw.so.1 (0x00007f2c6074b000)
        libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00007f2c60534000)
        libcap-ng.so.0 => /lib64/libcap-ng.so.0 (0x00007f2c6032e000)
        libxcb.so.1 => /lib64/libxcb.so.1 (0x00007f2c60105000)
        libbz2.so.1 => /lib64/libbz2.so.1 (0x00007f2c5fef5000)
        libpng15.so.15 => /lib64/libpng15.so.15 (0x00007f2c5fcca000)
        libattr.so.1 => /lib64/libattr.so.1 (0x00007f2c5fac4000)
        libelf.so.1 => /lib64/libelf.so.1 (0x00007f2c5f8ac000)
```

copy lib to /home/flask/app/env/lib

in user flask ... copy it...
```
ldd /usr/bin/Xvfb > /tmp/a
awk '{print "cp "$3" /home/flask/app/env/lib/."}' /tmp/a > /tmp/b
sh /tmp/b
rm -rf /tmp/a /tmp/b
cp /lib64/ld-linux-x86-64.so.2 /home/flask/app/env/lib/.
```
 
 when reconfig or edit app.py need restart flaskapp
```
systemctl restart flaskapp.service
```


