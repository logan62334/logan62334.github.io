---
title: 'Python | 获取iOS设备信息的轻量级框架'
date: 2017-12-17 22:48:52
tags: Python iOS
category: Python iOS
---
今天接着上一篇[Python | 获取Android设备信息的轻量级框架](http://mp.weixin.qq.com/s?__biz=MzIyMzE3MTY3MA==&mid=2651317842&idx=1&sn=35b85e6bac55fa6bf2d0b855698e5cf8&chksm=f3d1036bc4a68a7d327275d14220b1edbb848ea234f4f35d3ed659765b7b1737275efbc0c3f3&scene=21#wechat_redirect)，来讲讲

如何通过Python实现一个轻量级的库来获取电脑上连接的iOS设备信息。

这个库只有一个文件，通过封装***libimobiledevice***命令实现，返回的是一个包含所有设备信息的标准json格式的列表方便解析，下面简单介绍一下：

## libimobiledevice命令封装
```
    @staticmethod
    def get_ios_devices():
        devices = []
        output = Shell.invoke('idevice_id -l')
        config_file = os.path.join(os.path.dirname(__file__), 'ios_mapping.json')
        with open(config_file, 'r') as f:
            config = json.loads(f.read())

        if len(output) > 0:
            udids = output.strip('\n').split('\t')
            for udid in udids:
                dic = {"os_type": 'iOS', "uid": udid}
                output = Shell.invoke('ideviceinfo -u %s -k ProductType' % udid)
                device_type = config[output.strip('\n')]
                brand = ''
                # -1表示找不到 0表示下标
                if device_type.find("iPhone") != -1:
                    brand = 'iPhone'
                elif device_type.find("iPad") != -1:
                    brand = 'iPad'
                elif device_type.find("iPod") != -1:
                    brand = 'iPod'

                dic['brand'] = brand
                dic['model'] = device_type

                output = Shell.invoke('ideviceinfo -u %s -k ProductVersion' % udid)
                dic['os_type'] = 'iOS'
                dic['os_version'] = output.strip('\n')
                dic['rom_version'] = output.strip('\n')

                output = Shell.invoke('idevicename -u %s' % udid)
                dic['device_name'] = output.strip('\n')
                devices.append(dic)
        return devices
```

## 设备信息数据结构
```
[
    {
        "uid": "xxxxxxxxxxxxxx1f8a4dcfaac1fd01",
        "rom_version": "11.0.3",
        "brand": "iPhone",
        "device_name": "马飞的 iPhone",
        "os_version": "11.0.3",
        "model": "iPhone6s",
        "os_type": "iOS"
    }
]
```

注：有时候会报Couldn't connect to lockdown这样的错误，执行下面命令即可：
```
$ brew uninstall ideviceinstaller
$ brew uninstall libimobiledevice
$ brew install --HEAD libimobiledevice
$ brew install ideviceinstaller
```
这个库我已经上传到Pypi仓库，源码在github：https://github.com/logan62334/python-apptoolkit，点击阅读原文可以访问

***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
