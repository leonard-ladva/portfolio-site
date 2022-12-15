<script setup lang="ts">
import { onMounted, ref } from 'vue'
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import { RectAreaLightHelper } from 'three/examples/jsm/helpers/RectAreaLightHelper';
// defineProps<{ msg: string }>()

const canvas = ref(null)

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000);

onMounted(() => {
	const canvas: HTMLCanvasElement | null = document.querySelector('#background');
	if ( canvas === null ) {
		throw new Error("Canvas element not found")
	}

	const renderer = new THREE.WebGL1Renderer({
		canvas: canvas 
	})

	renderer.setPixelRatio( window.devicePixelRatio );
	renderer.setSize( window.innerWidth, window.innerHeight );
	camera.position.setZ(30);


	const loader = new GLTFLoader();
	loader.load( 'src/assets/tiny_room.glb', function ( gltf ) {
		gltf.scene.rotateY(Math.PI / 2)
		scene.add( gltf.scene );
	}, undefined, function ( error ) {
		console.error( error );
	} );

	// const axesHelper = new THREE.AxesHelper(50)
	// scene.add(axesHelper)
	
	const light = new THREE.RectAreaLight( 0xffffff, 5, 10, 10 );
	light.rotateX(- Math.PI / 2);
	light.position.set( 2, 7, 2 );
	scene.add( light );
	// light.add(new RectAreaLightHelper(light));

	const light2 = new THREE.RectAreaLight( 0xffffff, 5, 10, 10 );
	light2.rotateX(- Math.PI / 2);
	light2.position.set( -7, 7, -7 );
	scene.add( light2 );
	// light2.add(new RectAreaLightHelper(light2));

	const controls = new OrbitControls( camera, renderer.domElement );

	renderer.setAnimationLoop(animate)

	function animate() {
		controls.update()
		renderer.render( scene, camera);
	}

	window.addEventListener('resize', function() {
		camera.aspect = window.innerWidth / window.innerHeight;
		camera.updateProjectionMatrix();
		renderer.setSize(window.innerWidth, window.innerHeight);
	})
})

</script>

<template>
	<canvas ref="canvas" id="background"></canvas>
</template>

<style scoped>
	#background {
		position: fixed;
		top: 0;
		left: 0;
	}

</style>