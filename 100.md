# 一个专业Fake的Python库

在程序开发测试过程中，你是否为绞尽脑汁编制测试数据而烦恼？在注册国外网站服务的时候，你可曾使用谷歌地图，找寻合适的注册地址？在编写爬虫程序的时候，你有无为填写适合的UA而犯难？

感谢生在意大利罗马的`Daniele Faraglia`，为我们带来一个专业“造假”的Python库 `Faker`。

## 安装
安装很简单，使用 pip。

> pip install faker



## 使用 `faker` 库

安装完毕后，导入 `Faker` 类并实例化。


```python
from faker import Faker
fake = Faker()
```

显示随机人名:


```python
for _ in range(10):
    print(fake.name())
```

    Christina Freeman
    Kimberly Hudson
    Marcus Rodriguez
    Nicholas Rivera
    David Larson
    Joshua Pennington
    Mark Gallegos
    Benjamin Burke
    Cory Nelson
    Natalie White


显示随机UA，也可指定浏览器，如 `chrome`


```python
print(fake.user_agent())
print()
print(fake.chrome())
```

    Mozilla/5.0 (Windows NT 6.0; ca-ES; rv:1.9.1.20) Gecko/2017-01-08 13:00:46 Firefox/3.8
    
    Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_12_3) AppleWebKit/5362 (KHTML, like Gecko) Chrome/33.0.840.0 Safari/5362


### 指定 locale 参数
在实例化 `Faker` 时可指定42个国家或地区，如 `en_GB` 代表英国。


```python
fake_gb = Faker(locale="en_GB")

for _ in range(2):
    print(fake_gb.address(), '\n')
```

    330 Ricky roads
    South Jay
    DN09 6ZL 
    
    Flat 6
    Aimee course
    Terryton
    E72 8TG 



## 使用命令行
安装和，在命令行执行 `faker --help`可以查看使用说明。如
```shell
$ faker address
4876 Gallegos Vista Apt. 382
Lake Christine, VA 92929
```

## 延伸阅读

更多功能和用法，请自行查阅文档，文档托管在[Read the Docs](1)。项目源代码托管在[GitHub](2)。

[1]: https://faker.readthedocs.io/en/latest/
[2]: https://github.com/joke2k/faker


**郑重声明：请勿用于非法用途。**