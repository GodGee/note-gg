## 准备

我们需要建立一个vue项目，这里我直接用vue-cli脚手架了。

## Part 1：引入three.js

项目文件夹里打开终端窗口，并运行：

```text
npm install --save three
```

在需要使用three.js的组件内引入

```js
import * as THREE from 'three'
```

## Part 2：创建容器

创建canvas标签，为3D渲染建立容器。

```html
<template>
  <div>
    <canvas id="three"></canvas>
  </div>
</template>
```

## Part 3：创建场景

Three.js依赖一些要素，第一是scene，第二是render，第三是carmea。

我们首先为Three.js创建一个scene:

```js
const scene = new THREE.Scene()
```

scene可以理解为我们将要渲染的环境、背景。我们可以给它换个颜色:

```js
scene.background = new THREE.Color('#eee')
```

也可以通过自定义的 [纹理(Texture)](https://link.zhihu.com/?target=https%3A//threejs.org/docs/index.html%23api/zh/textures/Texture) 来为场景设置背景贴图。



接下来我们拿到canvas:

```js
const canvas = document.querySelector('#three')
```

创建一个WebGLRenderer，将canvas和[配置参数](https://link.zhihu.com/?target=https%3A//threejs.org/docs/index.html%23api/zh/renderers/WebGLRenderer)传入:

```js
const renderer = new THREE.WebGLRenderer({ canvas, antialias: true })
```



最后我们来创建一个camera用来观看场景里的内容,Three.js提供多种相机，比较常用的是PerspectiveCamera（透视摄像机）以及OrthographicCamera （正交投影摄像机）。

透视相机用来模拟人眼所看到的景象，物体的大小会受远近距离的影响，它是3D场景的渲染中使用得最普遍的投影模式。

而正交投影摄像机，不具有透视效果，即物体的大小不受远近距离的影响。

这里我们使用PerspectiveCamera（透视摄像机）:

```js
const camera = new THREE.PerspectiveCamera(50, window.innerWidth / window.innerHeight, 0.1, 1000)
```

PerspectiveCamera( fov : Number, aspect : Number, near : Number, far : Number )具有四个参数:

- fov — 摄像机视锥体垂直视野角度。可以理解为人类的视野广度。

- aspect — 摄像机视锥体横纵比。渲染结果的横向尺寸和纵向尺寸的比值，这里我们使用的是 浏览器窗口的宽高比。

- near — 摄像机视锥体近端面。一切比近面更近的事物将不被渲染。

- far — 摄像机视锥体远端面。一切比远面更远的事物将不被渲染，但是设置过大可能会影响性能。

  这些参数一起定义了摄像机的视锥体。

![img](https://pic2.zhimg.com/80/v2-3f2261288cde6a9632e2fef2183c0295_720w.jpg)PerspectiveCamera（透视摄像机）

生成的camera默认是放在中心点(0,0,0)的，但这是待会模型要放的位置，因此，我们把摄像机挪个位置:

```js
camera.position.z = 10
```



Three.js 需要一个动画循环函数，Three.js 的每一帧都会执行这个函数。

```js
function animate() {
  renderer.render(scene, camera)
  requestAnimationFrame(animate)
}
animate()
```



我们的场景的准备工作已经做好了，现在是一片灰色，没有物体。vue组件代码是这样的:

```js
<template>
  <div>
    <canvas id="three"></canvas>
  </div>
</template>

<script>
import * as THREE from 'three'
export default {
  mounted() {
    this.initThree()
  },
  methods: {
    initThree() {
      const scene = new THREE.Scene()
      scene.background = new THREE.Color('#eee')
      const canvas = document.querySelector('#three')
      const renderer = new THREE.WebGLRenderer({ canvas, antialias: true })
      const camera = new THREE.PerspectiveCamera(
        50,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      )
      camera.position.z = 10

      function animate() {
        renderer.render(scene, camera)
        requestAnimationFrame(animate)
      }
      animate()
    },
  },
}
</script>

<style scoped>
#three {
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
}
</style>
```

## Part 4 引入3D模型

官网介绍说three.js的核心专注于3D引擎最重要的组件。其它很多有用的组件 - 如控制器（control）、加载器（loader）以及后期处理效果（post-processing effect）这些就需要我们单独去引入。

如果我们想要加载外部3D模型，那么就需要用到加载器（loader），而Three.js提供的加载器又有好多种类型，分别可以加载不同的文件格式，其中官方比较推荐的是glTF格式，那么我们这里就使用glTF加载器：

```text
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
```

当然你也可以选择FBX、OBJ或者COLLADA等其他加载器，如果你是npm安装的three.js，可以去node_modules/three/examples/jsm/loaders 目录下查看更多支持的加载器。



加载器有了，现在我们还没有模型。可以去搜索一些免费的3D模型素材下载，当然你也可以自己做，我是去 [Sketchfab](https://link.zhihu.com/?target=https%3A//sketchfab.com/feed) 下载的，如果对3D模型的制作感兴趣也可以学习一下，推荐一个免费的软件[Blender](https://link.zhihu.com/?target=https%3A//www.blendercn.org/download) 。

![img](https://pic2.zhimg.com/80/v2-6f2c82baf91abcbe015af94406114bfd_720w.jpg)

萨勒芬妮。下载好后，解压，放进项目文件的public目录。

声明一个加载器，加载我们下载的模型，并把它添加到场景中，在animate函数上面添加代码:

```js
    const gltfLoader = new GLTFLoader()
      gltfLoader.load('/seraphine/scene.gltf', (gltf) => {
        var model = gltf.scene
        scene.add(model)
      })
```

刷新页面，场景里有了模糊的黑色的小人，这是因为我们还没有给她添加纹理。

![img](https://pic2.zhimg.com/80/v2-a933d3160fa926b789eacb8af8a4df05_720w.jpg)

来给她上个色:

```js
      gltfLoader.load('/seraphine/scene.gltf', (gltf) => {
        let model = gltf.scene

        //添加这段代码
        //遍历模型每部分
        model.traverse((o) => {
          //将图片作为纹理加载
          let explosionTexture = new THREE.TextureLoader().load(
            '/seraphine/textures/Mat_cwfyfr1_userboy17.bmp_diffuse.png'
          )
          //调整纹理图的方向
          explosionTexture.flipY = false
          //将纹理图生成基础网格材质(MeshBasicMaterial)
          const material = new THREE.MeshBasicMaterial({
            map: explosionTexture,
          })
          //给模型每部分上材质
          o.material = material
        })

        scene.add(model)
      })
```

好了，现在刷新下，她变成这样了，可以看到已经有颜色了，但还是很糊:

![img](https://pic4.zhimg.com/80/v2-7b8b996bb392bd6d467070470d0df7fb_720w.jpg)

原因是设备的物理像素分辨率与CSS像素分辨率的比值的问题，我们的canvas绘制出来后图片因为高清屏设备的影响，导致图片变大，然而我们在浏览器的渲染窗口并没有变大，因此图片会挤压缩放使得canvas画布会变得模糊。

修改它我们要用到devicePixelRatio这个属性，MDN解释：

> 此属性返回当前显示设备的物理像素分辨率与CSS像素分辨率的比值。该值也可以被解释为像素大小的比例：即一个CSS像素的大小相对于一个物理像素的大小的比值。

添加函数:

```js
     function resizeRendererToDisplaySize(renderer) {
        const canvas = renderer.domElement
        var width = window.innerWidth
        var height = window.innerHeight
        var canvasPixelWidth = canvas.width / window.devicePixelRatio
        var canvasPixelHeight = canvas.height / window.devicePixelRatio

        const needResize = canvasPixelWidth !== width || canvasPixelHeight !== height
        if (needResize) {
          renderer.setSize(width, height, false)
        }
        return needResize
      }
```

在animate函数内调用它：

```js
      function animate() {
        controls.update()
        renderer.render(scene, camera)
        requestAnimationFrame(animate)

        //添加下面代码
        if (resizeRendererToDisplaySize(renderer)) {
          const canvas = renderer.domElement
          camera.aspect = canvas.clientWidth / canvas.clientHeight
          camera.updateProjectionMatrix()
        }

      }
```

![img](https://pic2.zhimg.com/80/v2-6df44682e7bda570c215b2311873b7d1_720w.jpg)

模型看起来好多了。

## Part 5 添加轨道控制器

现在嘛，就只能看到萨勒芬妮的侧脸，我想康康正脸怎么办。

引入轨道控制器:

```js
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
```

创建它,可以写在animate函数上面:

```js
const controls = new OrbitControls(camera, renderer.domElement)
```

给它加点阻尼感，更真实点，泥可以对比下加不加的区别:

```js
controls.enableDamping = true
```

最后在animate函数里调用它，要写在 renderer.render(scene, camera)前面啊:

```js
controls.update()
```

现在就可以拉近点看脸了:

![img](https://pic3.zhimg.com/80/v2-28ae15f883d0b14a11c56e4801a69176_720w.jpg)

## Part 6 添加光与影

还是先加个地板叭，不然影子没地方投。Three.js里物体（一般叫网格Mesh）由两部分构成，一是它的形状，二是它的材质，我们给地板创建它们:

```js
let floorGeometry = new THREE.PlaneGeometry(3000, 3000)
let floorMaterial = new THREE.MeshPhongMaterial({color: 0xff0000})
```

平面几何体，PlaneGeometry(width : Float, height : Float, widthSegments : Integer, heightSegments : Integer)

> width — 平面沿着X轴的宽度。默认值是1。
> height — 平面沿着Y轴的高度。默认值是1。
> widthSegments — （可选）平面的宽度分段数，默认值是1。
> heightSegments — （可选）平面的高度分段数，默认值是1。

Phong网格材质(MeshPhongMaterial)

> 是一种用于具有镜面高光的光泽表面的材质。

生成Mesh，并添加到场景中：

```js
let floor = new THREE.Mesh(floorGeometry, floorMaterial)
floor.rotation.x = -0.5 * Math.PI
floor.receiveShadow = true
floor.position.y = -0.001
scene.add(floor)
```

再给他调整下位置，让他水平放置在萨勒芬妮的脚下，并让地板可以接收投影。

![img](https://pic1.zhimg.com/80/v2-e7c28e3876e3f9eb378e73b8cf30694c_720w.jpg)

可是现在是黑的，跟我们加的颜色不一样，那是因为没有光，Three.js提供的光源有很多种，有的可以产生阴影（平行光，点光源等），有的不行（半球光等）:

先添加个平行光:

```js
      const dirLight = new THREE.DirectionalLight(0xffffff, 0.6)
      //光源等位置
      dirLight.position.set(-10, 8, -5)
      //可以产生阴影
      dirLight.castShadow = true
      dirLight.shadow.mapSize = new THREE.Vector2(1024, 1024)
      scene.add(dirLight)
```

平行光一般用来模拟太阳光，DirectionalLight( color : Integer, intensity : Float )

> color - (可选参数) 16进制表示光的颜色。 缺省值为 0xffffff (白色)。
> intensity - (可选参数) 光照的强度。缺省值为1。

现在地板有颜色了，还可以添加多个光源，让场景看起来更真实:

```js
      const hemLight = new THREE.HemisphereLight(0xffffff, 0xffffff, 0.6)
      hemLight.position.set(0, 48, 0)
      scene.add(hemLight)
```

半球光光源直接放置于场景之上，光照颜色从天空光线颜色渐变到地面光线颜色。HemisphereLight( skyColor : Integer, groundColor : Integer, intensity : Float )

> skyColor - (可选参数) 天空中发出光线的颜色。 缺省值 0xffffff。
> groundColor - (可选参数) 地面发出光线的颜色。 缺省值 0xffffff。
> intensity - (可选参数) 光照强度。 缺省值 1。

想要产生影子，还需要在renderer下添加:

```js
      const renderer = new THREE.WebGLRenderer({ canvas, antialias: true })
      //加这句
      renderer.shadowMap.enabled = true;
```

以及:

```js
        model.traverse((o) => {
          //将图片作为纹理加载
          let explosionTexture = new THREE.TextureLoader().load(
            '/seraphine/textures/Mat_cwfyfr1_userboy17.bmp_diffuse.png'
          )
          //调整纹理图的方向
          explosionTexture.flipY = false
          //将纹理图生成基础网格材质(MeshBasicMaterial)
          const material = new THREE.MeshBasicMaterial({
            map: explosionTexture,
          })
          //给模型每部分上材质
          o.material = material

          //加这句，让模型等每个部分都能产生阴影
          if (o.isMesh) {
            o.castShadow = true
            o.receiveShadow = true
          }

        })
```

现在就有影子啦！

![img](https://pic4.zhimg.com/80/v2-2636f3167381e9a3fe55c0f24b4f79cf_720w.jpg)

最后最后最后，给场景加个雾叭:

```js
const scene = new THREE.Scene()
scene.background = new THREE.Color('#eee')
//在代码上面声明场景等下面加这句:
scene.fog = new THREE.Fog('#eee', 20, 100)
```

![img](https://pic1.zhimg.com/80/v2-da55c06ab6d8b25c5eefcd6507aee520_720w.jpg)



萨勒芬妮演示

OK，完事。



喔喔代码忘放了:

```js
<template>
  <div>
    <canvas id="three"></canvas>
  </div>
</template>

<script>
import * as THREE from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

export default {
  mounted() {
    this.initThree()
  },
  methods: {
    initThree() {
      const scene = new THREE.Scene()
      scene.background = new THREE.Color('#eee')
      scene.fog = new THREE.Fog('#eee', 20, 100)

      const canvas = document.querySelector('#three')
      const renderer = new THREE.WebGLRenderer({ canvas, antialias: true })
      renderer.shadowMap.enabled = true

      const camera = new THREE.PerspectiveCamera(
        50,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      )
      camera.position.z = 10

      const gltfLoader = new GLTFLoader()
      gltfLoader.load('/seraphine/scene.gltf', (gltf) => {
        let model = gltf.scene
        //遍历模型每部分
        model.traverse((o) => {
          //将图片作为纹理加载
          let explosionTexture = new THREE.TextureLoader().load(
            '/seraphine/textures/Mat_cwfyfr1_userboy17.bmp_diffuse.png'
          )
          //调整纹理图的方向
          explosionTexture.flipY = false
          //将纹理图生成基础网格材质(MeshBasicMaterial)
          const material = new THREE.MeshBasicMaterial({
            map: explosionTexture,
          })
          //给模型每部分上材质
          o.material = material
          if (o.isMesh) {
            o.castShadow = true
            o.receiveShadow = true
          }
        })
        scene.add(model)
      })

      const hemLight = new THREE.HemisphereLight(0xffffff, 0xffffff, 0.6)
      hemLight.position.set(0, 48, 0)
      scene.add(hemLight)

      const dirLight = new THREE.DirectionalLight(0xffffff, 0.6)
      //光源等位置
      dirLight.position.set(-10, 8, -5)
      //可以产生阴影
      dirLight.castShadow = true
      dirLight.shadow.mapSize = new THREE.Vector2(1024, 1024)
      scene.add(dirLight)

      let floorGeometry = new THREE.PlaneGeometry(8000, 8000)
      let floorMaterial = new THREE.MeshPhongMaterial({
        color: 0x857ebb,
        shininess: 0,
      })

      let floor = new THREE.Mesh(floorGeometry, floorMaterial)
      floor.rotation.x = -0.5 * Math.PI
      floor.receiveShadow = true
      floor.position.y = -0.001
      scene.add(floor)

      const controls = new OrbitControls(camera, renderer.domElement)
      controls.enableDamping = true

      function animate() {
        controls.update()
        renderer.render(scene, camera)
        requestAnimationFrame(animate)

        if (resizeRendererToDisplaySize(renderer)) {
          const canvas = renderer.domElement
          camera.aspect = canvas.clientWidth / canvas.clientHeight
          camera.updateProjectionMatrix()
        }
      }
      animate()

      function resizeRendererToDisplaySize(renderer) {
        const canvas = renderer.domElement
        var width = window.innerWidth
        var height = window.innerHeight
        var canvasPixelWidth = canvas.width / window.devicePixelRatio
        var canvasPixelHeight = canvas.height / window.devicePixelRatio

        const needResize =
          canvasPixelWidth !== width || canvasPixelHeight !== height
        if (needResize) {
          renderer.setSize(width, height, false)
        }
        return needResize
      }
    },
  },
}
</script>

<style scoped>
#three {
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
}
</style>
```