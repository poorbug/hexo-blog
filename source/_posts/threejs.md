---
title: Three.js 起手式
date: 2016-12-06
tags:
- Three.js
- starter
---

## 题外话：

起手式，这个词第一次是从金庸的小说中看到的，通常用来向对手表示礼貌与尊敬，并不具备攻击性；大部分门派的基本功第一招都是起手式，因此也用来表示入门的第一步。

---

## bg:
1. 年会抽奖系统
2. 头像滚动
3. Powered by Three.js Based on WebGL
---

## 相关资料：
1. [ Three.js 基础教程](http://www.hewebgl.com/article/articledir/1) // 比较入门的教程，非常好理解，但是 demo 里头的一些方法已经废弃，建议参照官网文档进行阅读
2. [ 官网 ](https://threejs.org/) // 官网并不是很适合入门，没有 tutorial，文档当字典用，demo 看看效果，源码也可以看点，但是不是很友好
3. [ Three.js 开发指南.pdf ](http://ohqua234y.bkt.clouddn.com/webgl/THREE.JS%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97.pdf) //  电子书，上面俩找不到的东西，可以上这里头翻翻
4. [淘宝团队 WebGL 技术储备指南](http://taobaofed.org/blog/2015/12/21/webgl-handbook/)  //有比较多的数学概念，不是很好理解，建议进阶阅读
---

## 正文
先上一个 Demo。概念的东西就不提了，在资料里头的教程里就能看到简单易懂的介绍。
首先撇开其他方面的依赖，直接回到最原始的开发环境中，也就是静态的 HTML 页面。

``` html
<!DOCTYPE html>
<html>
<head>
    <title>Three.js Demo</title>
    <style>
        html, body { margin:0;overflow: hidden; }
        canvas { width: 100%; height: 120% }
    </style>
</head>
<body>
<script src="./three.min.js" ></script>
<script>
    var scene, camera, renderer;

    init();

    function init(){
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 );
        renderer = new THREE.WebGLRenderer();

        camera.position.z = 0;
        renderer.setSize( window.innerWidth, window.innerHeight );

        var geometry = new THREE.PlaneGeometry( 128, 128, 1, 1);
        var material = new THREE.MeshBasicMaterial( {color: 0xfffff} );
        var plane = new THREE.Mesh( geometry, material );
        plane.position.z = -500;
        scene.add( plane );

        document.body.appendChild( renderer.domElement );
        renderer.render( scene, camera );
    }
</script>
</body>
</html>
```

在这里头画了一个正方形（只是一个面）， 调好 camera 与物体之间的距离，就能看到这个正方形。
让场景动起来的方法有两种，
1. 移动 camera ， 让场景相对动起来。
2. 移动场景里头的物体，这是场景真正动起来。

现在我们要让场景动起来，下面 Demo 采用的是第一种方法，让 camera 动起来。在代码里头加入 animate 方法，下面这个 Demo 只改变了 z 轴的值，使 camera 前后移动。
`requestAnimationFrame` 方法不明白自行 Google (之前看过一个很好的博文，找不着了)。

``` javascript
<script>
    var scene, camera, renderer;

    init();
    animate(); //初始化后执行 animate 方法

    function init(){
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 );
        renderer = new THREE.WebGLRenderer();

        camera.position.z = 0;
        renderer.setSize( window.innerWidth, window.innerHeight );

        var geometry = new THREE.PlaneGeometry( 128, 128, 1, 1);
        var material = new THREE.MeshBasicMaterial( {color: 0xfffff} );
        var plane = new THREE.Mesh( geometry, material );
        plane.position.z = -500;
        scene.add( plane );

        document.body.appendChild( renderer.domElement );
        renderer.render( scene, camera );
    }

    //增加了 animate 方法
    function animate(){
        requestAnimationFrame( animate );
        camera.position.z -= 5;
        //下面代码保证每次调用 animate 都重新渲染
        renderer.render( scene, camera );
    }THREE.ImageUtils.loadTexture

</script>
```

后来我又在 init 里头往场景里添加了许多物体，然后也是通过移动 camera 来使这些物体看起来像是在往前飞或者往后飞。
接下来我要给这个正方形贴上纹理，也就是把这个蓝色的正方形变成我想要的照片（也就是抽奖系统里的头像）。

教程里头使用是 `THREE.ImageUtils.loadTexture` 这个方法，但是实际应用后发现这个方法已经被废弃，后经过多方考证后我使用了 `THREE.TextureLoader` 对象里的 load 方法来载入纹理图片。
代码如下（我把 animate 方法去掉了）：

``` javascript
<script>
    var scene, camera, renderer;
    init();
    function init(){
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 );
        renderer = new THREE.WebGLRenderer();
        camera.position.z = 0;
        renderer.setSize( window.innerWidth, window.innerHeight );

        var loader = new THREE.TextureLoader();
        loader.load( "logo.png", function(texture){
                var geometry, material, plane;
                geometry = new THREE.PlaneGeometry( 128, 128, 1, 1);
                material = new THREE.MeshBasicMaterial( {map:texture} );
                // var material = new THREE.MeshBasicMaterial( {color: 0xfffff} );
                plane = new THREE.Mesh( geometry, material );
                plane.position.z = -500;
                scene.add( plane );
                document.body.appendChild( renderer.domElement );
                renderer.render( scene, camera );
            },
            function ( xhr ) {
                // onProcess 方法
                console.log( (xhr.loaded / xhr.total ) + '% loaded' );
            },
            function ( xhr ) {
                // onError 方法
                console.log( 'An error happened' );
            }
        );
    }
</script>
```

上面这个代码直接访问是会有问题的。
以踩完坑的角度来看这个问题。这个坑有两个影响因素：
1. 本地访问 THML 文件（file:///Applications/MAMP/htdocs/lottery/pic.html）  与  服务器访问 HTML 文件（http://localhost:8888/lottery/pic.html）
2. 相对路径访问纹理纹理图片  与  绝对路径访问纹理图片

第一种情况： 本地访问 HTML 文件 ＋ 相对路径访问纹理图片
console 中打出了两条日志：
- three.min.js:606 XMLHttpRequest cannot load file:///Users/poorbug/Desktop/lottery/logo.png. Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https, chrome-extension-resource.
- An error happened

跨域错误，于是我把图片放到七牛上。

第二种情况：本地访问 HTML 文件 ＋ 绝对路径( url )访问纹理图片
修改 load 方法中的第一个参数( url )；此时从 console 与 network 都可以看出 load 方法已经成功加载图片。但同时也爆出了一个错误：
- three.min.js:119 DOMException: Failed to execute 'texImage2D' on 'WebGLRenderingContext': Tainted canvases may not be loaded.(…)

纹理并没有成功渲染上，还是有问题，在这一块卡了非常久。

可能是从跨域问题获得了灵感。我把本地服务器启动了。

第三种情况：服务器访问 HTML 文件 ＋ 本地访问纹理图片
通过 localhost 访问 HTML 文件，则纹理能够成功贴上(不管是本地访问纹理图片还是通过服务器访问纹理图片)。

此时，完整代码如下（经过稍微加工，代码与上面 Demo 不完全一致）：
``` html
<!DOCTYPE html>
<html>
<head>
    <title>lottery</title>
    <style>
        html, body{margin:0;overflow: hidden;}
        canvas { width: 100%; height: 120% }
    </style>
    <script src="./three.min.js" ></script>
</head>
<body>
<script>
    var scene, camera, renderer;
    var step = 20;
    var len = 5;
    var dis = 500;

    init();
    animate();

    function init() {
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 1, 10000 );
        renderer = new THREE.WebGLRenderer();
        camera.position.z = -dis;
        renderer.setSize( window.innerWidth, window.innerHeight );

        var loader = new THREE.TextureLoader();
        loader.load( "logo.png", function(texture){
                var geometry, material, plane;
                for(var i=1; i<=len; i++){
                    geometry = new THREE.PlaneGeometry( 128, 128, 1, 1);
                    material = new THREE.MeshBasicMaterial( {map:texture} );
                    // var material = new THREE.MeshBasicMaterial( {color: 0xfffff} );
                    plane = new THREE.Mesh( geometry, material );
                    plane.position.z = i * dis;
                    scene.add( plane );
                }
                document.body.appendChild( renderer.domElement );
                renderer.render( scene, camera );
            },
            function ( xhr ) {
                console.log('size = '+ xhr.loaded)
                console.log( (xhr.loaded / xhr.total * 100) + '% loaded' );
            },
            // Function called when download errors
            function ( xhr ) {
                console.log( 'An error happened' );
            }
        );
    }
    function animate() {
        if(camera.position.z == (len + 1)*dis){
            // 所有照片都投入后，停留3秒，反向滚动
            console.log('=.'+camera.position.z)
            step = -step;
            setTimeout(animate, 3000)
        }else if(camera.position.z % dis == 480 && step > 0 && camera.position.z != (len + 1) * dis - step){
            // 每个照片会停留500毫秒
            console.log('-.'+camera.position.z)
            setTimeout(animate, 500)
        }else{
            requestAnimationFrame( animate );
        }
        camera.position.z += step;
        renderer.render( scene, camera );
    }
</script>
</body>
</html>
```

## 总结
---
经过几天的学习，发现 Three.js 的教程实在是不多，有不少还有点过时。官网的文档又没有教程，demo 晦涩难懂，简直不忍多看。因此学习这个还是需要自己花点时间琢磨；参考网上的资料，自己动手试一试，不轻松，但是会有收获。

That's all.
