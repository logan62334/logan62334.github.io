---
title: 'Python | 获取Android设备信息的轻量级框架'
date: 2017-12-17 22:48:52
tags: Python Android
category: Python Android
---
今天跟大家分享一下，如何通过Python实现一个轻量级的库来获取电脑上连接的Android设备信息，为什么说轻量呢因为整个库也就4KB，相比其他诸如Appetizer这样动辄就8MB多的库要轻很多，而且也基本满足项目中的需求。

这个库只有一个文件，通过封装Android的ADB命令实现，返回的是一个包含所有设备信息的标准json格式的列表方便解析，下面简单介绍一下：

### 检查环境变量
```
# 判断是否设置环境变量ANDROID_HOME
if "ANDROID_HOME" in os.environ:
    command = os.path.join(
        os.environ["ANDROID_HOME"],
        "platform-tools",
        "adb")
else:
    raise EnvironmentError(
        "Adb not found in $ANDROID_HOME path: %s." %
        os.environ["ANDROID_HOME"])
```

### 命令执行
```
class Shell:
    def __init__(self):
        pass

    @staticmethod
    def invoke(cmd):
        output, errors = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE).communicate()
        o = output.decode("utf-8")
        return o
```

### ADB命令封装
```
class ADB(object):
    """
      参数:  device_id
    """

    def __init__(self, device_id=""):

        if device_id == "":
            self.device_id = ""
        else:
            self.device_id = "-s %s" % device_id

    def adb(self, args):
        cmd = "%s %s %s" % (command, self.device_id, str(args))
        return Shell.invoke(cmd)

    def shell(self, args):
        cmd = "%s %s shell %s" % (command, self.device_id, str(args),)
        return Shell.invoke(cmd)

    def get_device_state(self):
        """
        获取设备状态： offline | bootloader | device
        """
        return self.adb("get-state").stdout.read().strip()

    def get_device_id(self):
        """
        获取设备id号，return serialNo
        """
        return self.adb("get-serialno").stdout.read().strip()

    def get_android_version(self):
        """
        获取设备中的Android版本号，如4.2.2
        """
        return self.shell(
            "getprop ro.build.version.release").strip()

    def get_sdk_version(self):
        """
        获取设备SDK版本号，如：24
        """
        return self.shell("getprop ro.build.version.sdk").strip()

    def get_product_brand(self):
        """
        获取设备品牌，如：HUAWEI
        """
        return self.shell("getprop ro.product.brand").strip()

    def get_product_model(self):
        """
        获取设备型号，如：MHA-AL00
        """
        return self.shell("getprop ro.product.model").strip()

    def get_product_rom(self):
        """
        获取设备ROM名，如：MHA-AL00C00B213
        """
        return self.shell("getprop ro.build.display.id").strip()
```

### 设备信息获取
```
class DeviceInfo:
    def __init__(self, uid, os_type, os_version, sdk_version, brand, model, rom_version):
        self.uid = uid
        self.os_type = os_type
        self.os_version = os_version
        self.sdk_version = sdk_version
        self.brand = brand
        self.model = model
        self.rom_version = rom_version


class Device:
    def __init__(self):
        pass

    @staticmethod
    def get_android_devices():
        android_devices_list = []
        android_devices_infos = []
        for device in Shell.invoke('adb devices').splitlines():
            if 'device' in device and 'devices' not in device:
                device = device.split('\t')[0]
                android_devices_list.append(device)
        for device_uid in android_devices_list:
            device_info = DeviceInfo(device_uid, "Android", ADB(device_uid).get_android_version(),
                                     ADB(device_uid).get_sdk_version(),
                                     ADB(device_uid).get_product_brand(), ADB(device_uid).get_product_model(),
                                     ADB(device_uid).get_product_rom())
            android_devices_infos.append(device_info.__dict__)
        return android_devices_infos
```

### 设备信息数据结构
```
[
    {
        "uid": "BY2WKN1519078327",
        "rom_version": "Che2-UL00 V100R001CHNC00B287",
        "brand": "Honor",
        "os_version": "4.4.2",
        "sdk_version": "19",
        "os_type": "Android",
        "model": "Che2-UL00"
    },
    {
        "uid": "GWY0217414001213",
        "rom_version": "MHA-AL00C00B213",
        "brand": "HUAWEI",
        "os_version": "7.0",
        "sdk_version": "24",
        "os_type": "Android",
        "model": "MHA-AL00"
    }
]
```

### 安装&更新
```
pip install -U apptoolkit
```

这个库我已经上传到Pypi仓库，源码在github：https://github.com/logan62334/python-apptoolkit

***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
