<script setup lang="ts">
import { onMounted, ref } from 'vue'
import * as THREE from 'three';

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

	renderer.render( scene, camera );
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