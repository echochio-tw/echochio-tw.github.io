---
layout: post
title: 百度 CDN 的 API 檢查域名是否有備案
date: 2019-12-11
tags: baidu cdn icp
---

解壓縮 ...bce-python-sdk-0.8.3.zip

檔案
```
bce-python-sdk-0.8.3/baidubce/services/cdn/cdn_client.py
```
加入
```
    def icp_domain(self, domain, config=None):
        """
        GET	/v2/domain/{domain}/icp	查询域名是否备案
        query a domain icp

        :param domain: the domain name
        :type domain: string
        :param config: None
        :type config: baidubce.BceClientConfiguration

        :return:
        :rtype: baidubce.bce_response.BceResponse
        """
        return self._send_request(
            http_methods.GET,
            '/domain/' + domain + '/icp',
            config=config)
```


這邊還可加入一些功能 .... 
```
    def config_get_domain(self, domain, config=None):
        """
        GET	/v2/domain/{domain}/config	获取指定加速域名配置的基本信息
        query a domain config

        :param domain: the domain name
        :type domain: string
        :param config: None
        :type config: baidubce.BceClientConfiguration

        :return:
        :rtype: baidubce.bce_response.BceResponse
        """
        return self._send_request(
            http_methods.GET,
            '/domain/' + domain + '/config',
            config=config)

    def certificates_get_domain(self, domain, config=None):
        """
        GET	/v2/{domain}/certificates	查询domain绑定的certificate
        query a domain icp

        :param domain: the domain name
        :type domain: string
        :param config: None
        :type config: baidubce.BceClientConfiguration

        :return:
        :rtype: baidubce.bce_response.BceResponse
        """
        return self._send_request(
            http_methods.GET,
            domain + '/certificates',
            config=config)
            
    def certificates_del_domain(self, domain, config=None):
        """
        DELETE	/v2/{domain}/certificates	删除domain绑定的certificate
        delete domain certificates

        :param domain: the domain name
        :type domain: string
        :param config: None
        :type config: baidubce.BceClientConfiguration

        :return:
        :rtype: baidubce.bce_response.BceResponse
        """
        return self._send_request(
            http_methods.DELETE,
            domain + '/certificates',
            config=config)    
```

要先編譯一下
```
python setup.py install
```

壓起來
```
zip -r bce-python-sdk-0.8.33.zip bce-python-sdk-0.8.33
```

移除舊版後裝上去
```
pip uninstall bce-python-sdk
pip install bce-python-sdk-0.8.33.zip
```

範例:
```
"""
Samples for cdn client.
"""

import os
import random
import string

import cdn_sample_conf
from baidubce import compat
from baidubce import exception
from baidubce.exception import BceServerError
from baidubce.services.cdn.cdn_client import CdnClient
from baidubce.services.cdn.cdn_stats_param import CdnStatsParam

import imp
import sys

imp.reload(sys)
if compat.PY2:
    sys.setdefaultencoding('utf8')

def test_icp_domain(c):
    """
    test icp domain
    """
    response = c.icp_domain('ap.jxaagajx.cn')
    print(response)

if __name__ == "__main__":
    import logging

    logging.basicConfig(level=logging.INFO)
    __logger = logging.getLogger(__name__)

    c = CdnClient(cdn_sample_conf.config)
    test_icp_domain(c)
```
