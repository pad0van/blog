# 如何用 `Node.Js` 和 `Puppeteer` 爬取网页

**原文**：[How To Scrap That Web Page With Node.Js And Puppeteer](https://coding.napolux.com/how-to-scrap-web-page-nodejs-puppeteer/)

**作者**：[Francesco Napoletano](https://medium.com/@napolux?source=post_header_lockup) **发表时间**：2018/8/17

**译者**：[陈 昌茂](https://juejin.im/user/59aabc9af265da249517aa6d) **发表时间：**2018/8/20

(**转载请注明出处**)



如果你像我一样，**有时非常急切地想要抓去某个网页**，得到可读格式的数据，或仅是需要这些数据用做其他目的。

> 我庄严发誓，我在干坏事。（非译者）

我在多次尝试以后（如： `Guzzle` 和 `BeautifulSoup` 等），终于找到了最优组合方案：

- Node.js（ 译者注：采用谷歌V8引擎开发的在服务器端运行JavaScript代码的跨平台运行环境 ）
- Puppeteer（ 译者注：木偶人，是谷歌浏览器的一个子项目，源代码托管在 [GitHub](https://github.com/GoogleChrome/puppeteer) ）
- A little Raspberry Pi where my scripts can run all day long.
- 用于全天侯跑脚本的一个黑莓派（译者注：貌似和正文无关）

`Puppeteer` 是一个 `Node` 代码库，基于 `DevTools` 协议，提供高级 API 自动化控制谷歌`Chrome` 或 `Chromium`浏览器。`Puppeteer` 默认以无界面方式运行，但也可以设置为有界面方式运行谷歌`Chrome` 或 `Chromium`浏览器。

**这意味着什么？**你可以在你的服务中运用一个谷歌`Chrome` 浏览器实例。是不是很酷～

下面我们一起来看下如何做。

### 设置运行环境

照例是打开终端，建立项目文件夹，在刚创建的文件夹运行命令  `npm init`。

命令执行后，文件夹中生成一个 `package.json` 的文件。执行命令 `npm i -S puppeteer` 安装 `Puppeteer`. 

> 小小的警告：安装 `Puppeteer` 过程中会下载完整版的谷歌`Chromium`浏览器到 `node_modules` 目录。

不用担心。自从 `1.7.0` 版后，谷歌发布了新的 `puppeteer-core` 安装包，默认不再自动下载谷歌`Chromium`浏览器。

如果你想尝鲜，执行命令 `npm i -S puppeteer-core` 即可。

> `puppeteer-core` 是 `Puppeteer` 的轻量级版本，复用本地已安装的浏览器，或者连接到远程浏览器。

运行环境准备好了，开干！

### 直接上代码，知其然

把下述代码复制保存到项目文件夹的 `index.js` 文件中。

```javascript
const puppeteer = require('puppeteer');
const URL = 'https://coding.napolux.com';

puppeteer.launch({ headless: true, args: ['--no-sandbox', '--disable-setuid-sandbox'] }).then(async browser => {
    const page = await browser.newPage();
    await page.setViewport({width: 320, height: 600})
    await page.setUserAgent('Mozilla/5.0 (iPhone; CPU iPhone OS 9_0_1 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Version/9.0 Mobile/13A404 Safari/601.1')

    await page.goto(URL, {waitUntil: 'networkidle0'});
    await page.waitForSelector('body.blog');
    await page.addScriptTag({url: 'https://code.jquery.com/jquery-3.2.1.min.js'})

    const result = await page.evaluate(() => {
        try {
            var data = [];
            $('h3.loop__post-title').each(function() {
                const url = $(this).find('a').attr('href');
                const title = $(this).find('a').attr('title')
                data.push({
                    'title' : title,
                    'url'   : url
                });
            });
            return data; // Return our data array
        } catch(err) {
            reject(err.toString());
        }
    });

    // let's close the browser
    await browser.close();

    // ok, let's log blog titles...
    for(var i = 0; i < result.length; i++) {
        console.log('Post: ' + result[i].title + ' URL: ' + result[i].url);
    }
    process.exit();
}).catch(function(error) {
    console.error('No way Paco!');
    process.exit();
});
```

这些代码实现了网站爬取功能。项目完整代码托管在 [GitHub](https://github.com/napolux/puppy)。

### 深挖一点点，知其所以然

上述代码会爬取我的博客站点中所有帖子的标题和链接。我们修改用户代理（UA）把爬取过程模拟成是一台老的 iPhone 手机在浏览。

为便于爬取工作，我们把 `jQuery` 注入到网页中，这样就可以方便地使用 CSS 选择器了。

我们逐行看一下代码：

- 第1-2行：调用 `Puppeteer` 模块，设置将要爬取的网站地址。
- 第4行：执行`Puppeteer`。请记住，我们正在`异步王国`，所有的操作均是异步的，否则就是同步操作，需等待操作结束再执行下一行代码。如你所见，配置参数不言自明，我们的脚本是在无界面方式的谷歌`Chromium` 浏览器中运行的。
- 第5-10行：浏览器启动，打开网页，设置网页按手机屏幕尺寸显示，设置浏览器使用伪造的UA。等待页面加载，直到 CSS 选择器 `body.blog` 出现。
- 第11行：如上所述，把 `jQuery` 注入到网页中。
- 第13-28行：我们使用 `jQuery` 魔法，抽取我们需要的数据。
- 第31-37行：关闭浏览器。打印数据。

在项目文件夹执行命令 `node index.js` ，你将看到如下内容输出：

```bash
Post: Blah blah 1? URL: https://coding.napolux.com/blah1/
Post: Blah blah 2? URL: https://coding.napolux.com/blah2/
Post: Blah blah 3? URL: https://coding.napolux.com/blah3/
```

### 文章摘要说明

欢迎来到爬虫世界。比你预想的简单，对不对？网页爬取是一个有争议的事情，请记住只爬取获得授权的网站。

可以爬取你的网站吗？作为[网站](https://coding.napolux.com)的主人，**我没有授权你这么做**。

给你留个作业吧：如何爬取使用了 `AJAX` 的网页！😉