## 进过

昨天苹果正式发布 macOS Mojave 。相信很多果粉在第一时间已经升级到最新版了。此次安装包足足有6G，下载倒是很快，安装可费了2个多小时，把一上午上班的宝贵时光都浪费掉了。

此次升级，网上介绍很多。比如暗黑模式、新增iOS上有的备忘录、股票和家庭应用，更方便的截屏。

但是，打开终端也发现了烦人的一幕。在执行brew等依赖ruby的各种命令时，下面这一行错误信息是不是蹦出来，挥之不去。

> Ignoring bigdecimal-1.3.2 because its extensions are not built.  Try: gem pristine bigdecimal --version 1.3.2

依照提示，执行 `gem pristine` 命令，提示报错。（此处犯了一个低级错误，后面会讲到）

没办法，祭上大杀器谷歌。一番搜索下来，近一月记录没有相关查询结果，近一年的查询结果也只有区区一屏。比较有用的信息时提醒升级 `Xcode` 和 `Command Line Tools` 。Xcode 在已经自动升级了。执行 `xcode-select --install` 安装命令行工具。

心想这下可以了吧，命令行工具已经是最新版本了，问题应该可以解决了。一试，问题依旧存在。是不是 ruby 不是最新版呢？执行命令更新 `sudo gem update`。到这是，突然想起，原来执行 `gem pristine` 命令的时候忘了 sudo。等升级完 gem 后，执行 `sudo gem pristine bigdecimal --version 1.3.2` ，问题立马解决。

简言之，看到上述错误，执行 `sudo gem pristine bigdecimal --version 1.3.2` 命令即可。

## 总结

从上述过程来看，问题可能不复杂，依照错误提示执行相应命令即可。依照惯性思维，离不开谷歌，不仔细看错误提示，导致走了这些冤枉路。

