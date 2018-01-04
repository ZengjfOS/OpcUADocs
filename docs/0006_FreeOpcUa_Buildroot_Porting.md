# FreeOpcUa Buildroot Porting

* FreeOpcUa Python版本是纯粹的Python软件包，所以不需要另外交叉编译，所以这里采用板上直接安装的方式进行处理；
* Buildroot不支持在线安装支持包，所以要分析setup.py文件依赖的软件包，需要提前安装好；

## cat setup.py 

```Python
from setuptools import setup, find_packages                             # 让Buildroot Python支持setuptools，Python内核要支持xml

import sys

install_requires = ["python-dateutil", "pytz", "lxml"]                  # 选择好这些依赖模块
if sys.version_info[0] < 3:
    install_requires.extend(["enum34", "trollius", "futures"])          # 版本小于3，要支持这些模块

setup(name="opcua",
      version="0.90.4",
      description="Pure Python OPC-UA client and server library",
      author="Olivier Roulet-Dubonnet",
      author_email="olivier.roulet@gmail.com",
      url='http://freeopcua.github.io/',
      packages=find_packages(),
      provides=["opcua"],
      license="GNU Lesser General Public License v3 or later",
      install_requires=install_requires,
      extras_require={
          'encryption': ['cryptography']                                # 额外需要支持的加密模块
      },
      classifiers=["Programming Language :: Python",
                   "Programming Language :: Python :: 3",
                   "Programming Language :: Python :: 2",
                   "Development Status :: 4 - Beta",
                   "Intended Audience :: Developers",
                   "Operating System :: OS Independent",
                   "License :: OSI Approved :: GNU Lesser General Public License v3 or later (LGPLv3+)",
                   "Topic :: Software Development :: Libraries :: Python Modules",
                   ],
      entry_points={'console_scripts':
                    [
                        'uaread = opcua.tools:uaread',
                        'uals = opcua.tools:uals',
                        'uabrowse = opcua.tools:uals',
                        'uawrite = opcua.tools:uawrite',
                        'uasubscribe = opcua.tools:uasubscribe',
                        'uahistoryread = opcua.tools:uahistoryread',
                        'uaclient = opcua.tools:uaclient',
                        'uaserver = opcua.tools:uaserver',
                        'uadiscover = opcua.tools:uadiscover',
                        'uacall = opcua.tools:uacall',
                    ]
                    }
      )

```

## Install

```Shell
# python setup.py install
```
