# 102Jupyter 笔记本文件版本控制问题及解决方案

## 引言

今天（10月21日）参加在上海壹号码头艺术酒店举办中国 Python 开发者大会(PyCon China 2018) 。会上多位 Pythoneer 的做了生动活泼的知识分享，如 [Jupyter](http://jupyter.org/)、[PyTorch](https://pytorch.org/) 、[pyenv](https://github.com/pyenv/pyenv)、[pyeth](https://github.com/HuangFJ/pyeth)、[vnpy](https://github.com/vnpy/vnpy)、[Numba](http://numba.pydata.org/) 等，知识干货很多。

## 问题提出

上午提问环节中有小伙伴问到 Jupyter 笔记本文件（扩展名：ipynb）版本控制问题。估计演讲者没碰到这个问题，或者没有好的结局方案，回答是确实不好解决，Jupyter 笔记本使用 JSON 文件格式存储。由于 JSON 格式的原因，Jupyter 笔记本稍有变动，Jupyter 笔记本文件（JSON 格式存储）会有较大的不同，Git 版本管理工具没法很好的做版本控制。

## 解决方案

作为一个 Jupyter 的使用者，前段时间我也碰到了这个问题，踩坑摸索一番，算是“圆满”解决了这个问题。

定义一下方案要解决的问题：”优雅地“用 Git 管理 Jupyter 笔记本的 ipynb 文件修改历史纪录。

解决方案有两个，我推荐第二个方案。

方案一：安装 [notedown](https://github.com/aaren/notedown) 插件，使 Jupyter 笔记本可以修改并运行 Markdown 格式的代码，换句话说，用 md 文件替代 ipynb 文件。（这个是从李沐教程中学来的，实践中坑比较多）

安装 notedown 插件，运行 Jupyter 笔记本并加载插件：

```shell
pip install https://github.com/aaren/notedown/tarball/master
jupyter notebook --NotebookApp.contents_manager_class='notedown.NotedownContentsManager'

# 也可以这样，不过版本比较低(1.4.2, 最新1.5.0)，兼容性也差
# pip install notedown
```

如果你希望每次运行 Jupyter 笔记本时默认开启 notedown 插件，可以参考下面步骤。

首先，执行下面命令生成  Jupyter 笔记本配置文件（如果已经生成可以跳过）。

```shell
jupyter notebook --generate-config
```

然后，将下面这一行加入到 Jupyter 笔记本配置文件的末尾（Linux/macOS 上一般在路径`~/.jupyter/jupyter_notebook_config.py`)

```python
c.NotebookApp.contents_manager_class = 'notedown.NotedownContentsManager'
```

之后，我们只需要运行`jupyter notebook`命令即可默认开启 notedown 插件。



方案二：安装 [nbstripout](https://github.com/kynan/nbstripout) 工具，使 Git 在保存 Jupyter 笔记本到 Git 版本库时“忽略”输出项，使 outputs 为空，同时保持工作区（working directory）不变。这样做一般不会有问题，其他人在 checkout 后只需要重新运行一次 Jupyter 笔记本即可查看输出内容。

```json
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "Using TensorFlow backend.\n"
     ]
    }
```

在 Git 项目所在目录中执行 nbstripout 完成配置即可，照常使用 git 各种命令，如 add、checkout、commit、diff 等。只有在确实修改了 code 后，才会产生变更，提交后生成版本纪录。

安装 nbstripout 工具。

```
nbstripout --install
```

也可以随时卸载 nbstripout 工具。

```
nbstripout --uninstall
```

这两个命令的实际作用是修改 `.git/config` 文件

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[filter "nbstripout"]
	clean = \"/Users/ccm/anaconda3/bin/python\" \"/Users/ccm/anaconda3/lib/python3.7/site-packages/nbstripout.py\"
	smudge = cat
	required = true
[diff "ipynb"]
	textconv = \"/Users/ccm/anaconda3/bin/python\" \"/Users/ccm/anaconda3/lib/python3.7/site-packages/nbstripout.py\" -t
```



总结一下，Jupyter 笔记本文件版本控制问题的关键点在于 ipynb 文件储存 outputs 输出，这部分往往是多变的、无需做版本控制的，解决了这个，也就解决了Jupyter 笔记本文件版本控制问题。



转载请注明出处，也欢迎与我交流。