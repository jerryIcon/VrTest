# 前言
最近看到b站up主MarsLUL做了个AR手办生成器，觉得很有意思，今天就来照着他的讲解来实现一下吧！
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145931_d0f432ab_7641703.png)
那么现在就来开始！

# 步骤：
### 第一步：搭建静态服务器
这里用Node.js开启一个静态服务器，实际上用Tomcat或者其他服务器也是可以的。
Node.js直接去官网下载然后直接一直点下一步安装就好了，这里就不附上安装教程了。
> ==首先==创建一个index.js来创建一个简单的静态服务器

![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145931_073bf682_7641703.png)

```javascript
var fs = require('fs');
var http = require('http');

//用http创建一个静态服务器
http.createServer(function(req, res) {
    fs.readFile(__dirname + req.url, function (err, data) {
        if(err) {
            res.writeHead(404);
            res.end(JSON.stringify(err));
            return;
        }
        res.writeHead(200);
        res.end(data);
    });
}).listen(8080);
```
我们往项目中随便放入一张图片
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145931_24b450a0_7641703.png)
然后在控制台执行`node index.js`
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145931_913046a0_7641703.png)
接着访问`localhost:8080/1.jpg`看是否可以访问
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145932_bb22dfec_7641703.png)
这样静态服务器就搭建完成啦！

### 第二步：下载模型
我们可以去[https://sketchfab.com](https://sketchfab.com)随便下载一个免费的模型
这里我下载一个鸣人的
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145932_5f67efa0_7641703.png)
记得一定要下载gltf格式的
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145932_41286d02_7641703.png)
在项目里创建static文件夹，在static下再创建gltf文件夹，然后将压缩包解压到这里面
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145932_0aad7317_7641703.png)

模型下载完毕！

### 第三步：编写应用
在static文件夹下创建js文件夹，放入提前在ar.js库下载的两个js文件`aframe.min.js`和`aframe-ar.js`，后面我会放到我的gitee仓库，大家可以去里面拿

![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145932_a22a1506_7641703.png)
然后创建`index.html`，编写页面
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145933_e95d9168_7641703.png)

```html
<!DOCTYPE html>
<html>
    <!--下载并运行AR.js-->
  <script src="static/js/aframe.min.js"></script>
  <!-- we import arjs version without NFT but with marker + location based support -->
  <script src="static/js/aframe-ar.js"></script>
  <body style="margin : 0px; overflow: hidden;">
    <!--a-scene会创建一个AR环境-->
    <a-scene embedded arjs>
        <!--a-marker是在相机中想要识别模型的标记-->
        <a-marker preset="hiro">
            <a-entity
            position="0 0.5 0"
            scale="1.5 1.5 1.5"
            rotation="-160 0 0"
            gltf-model="static/gltf/naruto/scene.gltf"
            ></a-entity>
        </a-marker>
      <a-entity camera></a-entity>
    </a-scene>
  </body>
</html>
```
最后只要让手机访问上这个页面就好啦！

### 第四步：内网穿透
这里我们使用ngrok可以很简单的做一个内网穿透，只要去ngrok官网下载ngrok客户端就好啦：[https://ngrok.com/download](https://ngrok.com/download)
打开后执行`ngrok http 8080`,
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145932_8d1eba5d_7641703.png)
这样我们就完成，用手机访问https的那个地址，后面加上/index.html就可以访问的到了！
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145933_2e0cdab1_7641703.png)
# 成果
接下来用手机访问`https://3bbdf92fb2a5.ngrok.io/index.html`，然后就可以打开啦~然后扫官方的这张识别图
![在这里插入图片描述](https://images.gitee.com/uploads/images/2020/0721/145933_dcc00ed6_7641703.png)
因为是这种简单内网穿透的方式，所以加载很慢，有可能访问进入这个页面需要1~2分钟，然后这个模型可能要等2-3分钟左右才会出来

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200721144808719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzcyMzg3Nw==,size_16,color_FFFFFF,t_70 =300x)
成功啦！是不是非常简单！

# gitee地址
最后附上gitee地址：







