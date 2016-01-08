---
title: Hello World
---
Welcome to [Hexo](http://hexo.io/)! This is your very first post. Check [documentation](http://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](http://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](http://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](http://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](http://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```


More info: [Deployment](http://hexo.io/docs/deployment.html)

[《使用Travis CI自动构建Hexo静态博客》](http://blog.coryphaei.com/2015/12/11/%E4%BD%BF%E7%94%A8Travis%20CI%E8%87%AA%E5%8A%A8%E6%9E%84%E5%BB%BAHexo%E9%9D%99%E6%80%81%E5%8D%9A%E5%AE%A2/)

两个需要注意的地方：
- 生成的 id_rsa 是输入空密码
- deploy https://github.com/yinjimmy/yinjimmy.github.io/blob/blog/_config.yml#L73
