<template>
  <div id="three"></div>
</template>
<script>
import * as THREE from 'three'

import { OrbitControls } from 'three-orbitcontrols-ts'

// 纹理图片
import boxwl from '../assets/box.png'
import tagwl from '../assets/tag.png'
import boxTopwl from '../assets/boxtop.png'
import jzxwl from '../assets/jzx.png'
export default {
  name: 'App',
  components: {},
  provide() {
    return {
      details: this
    }
  },
  mounted() {
    this.render3d()
  },
  methods: {
    render3d() {
      //1.场景
      var scene = new THREE.Scene()
      scene.background = new THREE.Color('#ffffff')
      //2.摄像机
      var camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 1000)
      // 摄像机摆放位置
      camera.position.set(500, 500, 500)
      camera.lookAt(0, 0, 0) //相机观察目标指向点

      // 其他内容部分       ==========  start  ==========
      this.renderContent(scene)

      // 其他内容部分       ==========   end   ==========

      // 3.渲染器
      var renderer = new THREE.WebGLRenderer()
      renderer.setSize(window.innerWidth, window.innerHeight)
      setTimeout(() => {
        renderer.render(scene, camera) //渲染
      }, 100)
      document.querySelector('#three').appendChild(renderer.domElement)

      // 控制器
      this.sceneControls(scene, camera, renderer)
    },
    renderContent(scene) {
      // 画坐标轴
      // this.drawAxes(scene)
      // 添加光源
      this.renderLight(scene)
      // 画一个平面
      // this.drawPlane(scene)
      // 画一个3D立体 正方形
      this.drawBox(scene, { l: 100, w: 100, h: 100 }, { x: 0, y: 0, z: 0 }, { id: '001' })
      // 集装箱
      this.drawContainer(scene, { l: 450, w: 200, h: 200 })
      // this.drawSphere(scene)
    },
    // 画坐标轴
    drawAxes(scene) {
      // 轴 rgb (r红色表示：x轴，g绿色表示y轴，b蓝色表示z轴)
      const axesHelper = new THREE.AxesHelper(200)
      scene.add(axesHelper)
    },
    renderLight(scene) {
      // 光源m
      const AmbientLight = new THREE.AmbientLight(0xffffff)
      scene.add(AmbientLight)
      // 点光
      var light = new THREE.PointLight('#ffffff', 0, 1)
      light.position.set(200, 200, 200) //default; light shining from top
      scene.add(light)
      // 平行光
      // const directionalLight = new THREE.DirectionalLight(0xffffff, 0.2)
      // scene.add(directionalLight)
    },
    drawPlane(scene) {
      const geometry = new THREE.PlaneGeometry(200, 150)
      let material = new THREE.MeshLambertMaterial({
        color: '#00ff00', //平面颜色 绿色
        opacity: 1
      })
      const plane = new THREE.Mesh(geometry, material)
      plane.position.set(0, 0, 0)
      scene.add(plane)
    },
    // 画单个箱子
    drawBox(scene, box, axes, info) {
      let { l, w, h } = box
      let { x, y, z } = axes
      let geometry = new THREE.BoxGeometry(l, w, h)
      // 箱子图片纹理
      let textureLoader = new THREE.TextureLoader()
      let plasterTexture = textureLoader.load(boxwl)

      let material = new THREE.MeshLambertMaterial({
        color: '#ce9e6a',
        // wireframe: true,
        map: plasterTexture,
        opacity: 1
      })

      // 根据箱子画出响应 标签
      this.drawTagPlane(scene, box, axes)
      // 箱子顶部封口
      this.drawTopPlane(scene, box, axes)
      let cube = new THREE.Mesh(geometry, material)
      cube.position.set(x, y, z)

      cube.boxType = 'BOX'
      cube.boxInfo = JSON.stringify(info)
      scene.add(cube)
    },
    // 画集装箱
    drawContainer(scene, box, axes = { x: 0, y: 0, z: 0 }) {
      let { l, w, h } = box
      let { x, y, z } = axes
      var JGeometry = new THREE.BoxGeometry(l, w, h)
      // 箱子图片纹理
      let textureLoader = new THREE.TextureLoader()
      let plasterTexture = textureLoader.load(jzxwl)
      var jMmaterial = new THREE.MeshLambertMaterial({
        color: '#00229',
        transparent: true,
        opacity: 0.6,
        map: plasterTexture
      })
      var cube = new THREE.Mesh(JGeometry, jMmaterial)
      cube.position.set(x, y, z) // 设置立方体坐标
      cube.custType = 'jzx'
      scene.add(cube)
    },
    // 创建一个平面 标签
    drawTagPlane(scene, box, axes) {
      let { l, h, w } = box
      let { x, y, z } = axes
      const geometry = new THREE.PlaneGeometry(20, 15)
      let textureLoader = new THREE.TextureLoader()
      let plasterTexture = textureLoader.load(tagwl)
      let material = new THREE.MeshLambertMaterial({
        map: plasterTexture,
        opacity: 1
      })
      const plane = new THREE.Mesh(geometry, material)
      plane.position.set(
        x + (l / 2 - 20) + 1,
        y - w / 2 + 10 + 1,
        z + h / 2 + 1.01
        // 100 - this.cabinetSize.h / 2 + 1
      ) // 偏移设置立方体坐标
      scene.add(plane)
    },
    // 创建一个平面 顶部封口
    drawTopPlane(scene, box, axes) {
      let { l, w, h } = box
      let { x, y, z } = axes
      const geometry = new THREE.PlaneGeometry(l, w)
      let textureLoader = new THREE.TextureLoader()
      let plasterTexture = textureLoader.load(boxTopwl)
      let material = new THREE.MeshLambertMaterial({
        color: '#ce9e6a',
        map: plasterTexture,
        opacity: 1
      })
      const plane = new THREE.Mesh(geometry, material)
      plane.rotateX(-Math.PI / 2) //旋转网格模型
      plane.position.set(x + 1, y + h / 2 + 0.01, z / 2 + 1) // 偏移设置立方体坐标
      scene.add(plane)
    },
    //  圆立体
    drawSphere(scene) {
      // 球体网格模型
      var geometry2 = new THREE.SphereGeometry(20, 20, 20)
      var material2 = new THREE.MeshLambertMaterial({
        color: 0xfdd500
      })
      var mesh2 = new THREE.Mesh(geometry2, material2) //网格模型对象Mesh
      // 设置 坐标
      mesh2.position.set(200, 100, 100)
      scene.add(mesh2)
    },
    sceneControls(scene, camera, renderer) {
      // 控制器渲染
      var controls = new OrbitControls(camera, renderer.domElement)
      controls.addEventListener('change', function () {
        // 直接渲染
        renderer.render(scene, camera) //渲染
      })
    }
  }
}
</script>
<style scopted>
#three {
  width: 100%;
  height: 100vh;
  background: #ccc;
}
</style>
