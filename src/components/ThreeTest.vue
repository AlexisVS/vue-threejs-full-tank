<template>
  <canvas ref="canvas"></canvas>
</template>

<script setup>
import { onMounted, ref } from "vue";
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

// variables
let canvas = ref(null),
  scene,
  renderer,
  gltfLoader = new GLTFLoader(),
  camera,
  tank,
  light;

/**
 * init three
 */
const init = () => {
  initRenderer();
  initScene();
  initCamera();
  initLoader();
  initLight();
  initTank();
  animate();
};

const initRenderer = () => {
  renderer = new THREE.WebGLRenderer({ canvas: canvas.value, antialias: true });
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(window.innerWidth, window.innerHeight);
};

const initScene = () => {
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0x000000);

  const axesHelper = new THREE.AxesHelper(5);
  scene.add(new THREE.AxesHelper());
};

// initOrbitControls

const initCamera = () => {
  camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.set(0, 1, 0);
  scene.add(camera);
};

const initLoader = () => {
  gltfLoader = new GLTFLoader();
};

const initLight = () => {
  light = new THREE.DirectionalLight(0xff0000, 100);
  light.position.set(2, 2, 2);
  scene.add(light);
};

const initTank = () => {
  gltfLoader.load("/public/object/cube/scene.gltf", (gltf) => {
    const tank = gltf.scene;
    scene.add(tank);
    const box = new THREE.Box3().setFromObject(tank);
  });
  console.log(tank);
  // scene.add(tank.scene);
};

const animate = () => {
  window.requestAnimationFrame(animate);
  renderer.render(scene, camera);
};

onMounted(() => {
  init();
});
</script>

<style scoped>
canvas {
  width: 100vw;
  height: 100vh;
}
</style>
