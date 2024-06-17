<template>
  <div id="container"></div>
</template>

<script setup>
import { onMounted } from 'vue';
import * as THREE from 'three'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
// 创建场景
const scene = new THREE.Scene()
// const sceneBackgroundColor = new THREE.Color(0xffffff)
// scene.background = sceneBackgroundColor

const cubeTexture = new THREE.CubeTextureLoader().load([
  '/2.jpg', '/2.jpg',
  '/2.jpg', '/2.jpg',
  '/2.jpg', '/2.jpg',
])

scene.background = cubeTexture


// 创建相机
const camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 1, 1000 );

camera.position.z = 5;
camera.position.x = 3;
camera.position.y = 3;

// 创建立方体（物体）
const geometry = new THREE.BoxGeometry( 1, 1, 1 ); 
// 创建物体材质
const material = new THREE.MeshBasicMaterial( { color: 0xffffff } ); 
// material.wireframe = true
// 创建纹理
// 立方纹理贴图

// 纹理贴图
const texture = new THREE.TextureLoader().load('/2.jpg')
material.map = texture
// material.envMap = cubeTexture
// 创建网格
const cube = new THREE.Mesh( geometry, material ); 
cube.position.y = 0.5
scene.add( cube )


// 相机始终看向物体
camera.lookAt(cube.position)

// 创建渲染器
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)

document.body.appendChild(renderer.domElement)
// 创建辅助网格
const size = 10;
const divisions = 10;

const gridHelper = new THREE.GridHelper( size, divisions );
scene.add( gridHelper );

// 轨道控制器
const controls = new OrbitControls( camera, renderer.domElement );
controls.autoRotate = true
controls.autoRotateSpeed = 1
// 开启阻尼，使其有重量感
controls.enableDamping = true
controls.dampingFactor = 0.01

// 坐标轴辅助线
const axesHelper = new THREE.AxesHelper( 5 );
scene.add( axesHelper );

// 灯光
const light = new THREE.AmbientLight( 0x404040 ); // 柔和的白光
scene.add( light );

function animate () {
  requestAnimationFrame(animate)
  cube.rotation.x += 0.01
  cube.rotation.y += 0.01
  controls.update()
  renderer.render(scene, camera)
}
animate();

function handleResize () {
  console.log(window.innerWidth, window.innerHeight)
  renderer.setSize(window.innerWidth, window.innerHeight)
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

</script>

<style>
html, body {
  margin: 0;
}
</style>
