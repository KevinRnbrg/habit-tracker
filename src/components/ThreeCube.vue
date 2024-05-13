<template>
    <div id="cube-container"></div>
</template>

<script>
import * as THREE from 'three';

export default {
    mounted() {
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera( 75, 1, 0.1, 1000 );
        const container = document.getElementById('cube-container');
        const renderer = new THREE.WebGLRenderer({ alpha: true });

        renderer.setSize( 300, 300 );
        container.appendChild( renderer.domElement );

        const size = 0.8;
        const halfSize = size / 2;
        const vertices = [
            new THREE.Vector3(-halfSize, -halfSize, -halfSize),
            new THREE.Vector3(halfSize, -halfSize, -halfSize),
            new THREE.Vector3(halfSize, -halfSize, halfSize),
            new THREE.Vector3(-halfSize, -halfSize, halfSize),
            new THREE.Vector3(-halfSize, halfSize, -halfSize),
            new THREE.Vector3(halfSize, halfSize, -halfSize),
            new THREE.Vector3(halfSize, halfSize, halfSize),
            new THREE.Vector3(-halfSize, halfSize, halfSize),
        ];
        const cubeG = new THREE.BufferGeometry().setFromPoints([
            vertices[0], vertices[1],
            vertices[1], vertices[2],
            vertices[2], vertices[3],
            vertices[3], vertices[0],
            vertices[4], vertices[5],
            vertices[5], vertices[6],
            vertices[6], vertices[7],
            vertices[7], vertices[4],
            vertices[0], vertices[4],
            vertices[1], vertices[5],
            vertices[2], vertices[6],
            vertices[3], vertices[7],
        ]);
        const cubeM = new THREE.LineBasicMaterial({ color: 0x000000 });
        const cube = new THREE.LineSegments(cubeG, cubeM);
        scene.add(cube);
        
        camera.position.z = 1.5;

        function animate() {
            requestAnimationFrame( animate );

            cube.rotation.x += 0.003;
            cube.rotation.y += 0.003;

            renderer.render( scene, camera );
        }

        animate();
    }
}
</script>
