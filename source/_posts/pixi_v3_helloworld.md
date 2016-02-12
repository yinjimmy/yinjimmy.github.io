---
title: pixi.js v3 - HelloWorld
tags: tech
date: 2016-02-11
---

从未进行过 web 开发的我摸一下 web 游戏，看看什么感觉。这些引擎和 native 的 quick-cocos2d-x 有多大区别。

---

[pixi.js](http://www.pixijs.com/) 是什么?

> 2D webGL renderer with canvas fallback


先来一个例子：

**NOTE:** 必须启动一个 http 服务器来测试自己的游戏，否则会遇到错误：

```
Uncaught SecurityError: Failed to execute 'texImage2D' on 'WebGLRenderingContext':
```

代码：

```
<!DOCTYPE HTML>
<html>
<head>
    <title>pixi.js helloworld</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background-color: #000000;
        }
    </style>
    <script src="http://cdn.bootcss.com/pixi.js/3.0.9/pixi.min.js"></script>
</head>
<body>
    <script type="text/javascript">

        var renderer = new PIXI.WebGLRenderer(800, 600, { backgroundColor : 0x1099bb } );

        document.body.appendChild(renderer.view);

        var stage = new PIXI.Container();

        var self = this;
        self.bunny = null;
        PIXI.loader.add('bunny', 'https://raw.githubusercontent.com/pixijs/examples/gh-pages/_assets/bunny.png').load(function (loader, resources) {
            // This creates a texture from a 'bunny.png' image.
            var bunny = new PIXI.Sprite(resources.bunny.texture);
            self.bunny = bunny;

            // Setup the position and scale of the bunny
            bunny.position.x = 400;
            bunny.position.y = 300;

            bunny.scale.x = 2;
            bunny.scale.y = 2;

            // // Add the bunny to the scene we are building.
            stage.addChild(bunny);

            animate();
        });

        function animate() {
            // start the timer for the next animation loop
            requestAnimationFrame(animate);

            // each frame we spin the bunny around a bit
            self.bunny.rotation += 0.01;

            // this is the main render call that makes pixi draw your container and its children.
            renderer.render(stage);
        }

    </script>
</body>
</html>

```
