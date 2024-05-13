<template>
    <div id="atom-container"></div>
</template>

<script>
import * as THREE from 'three';

export default {
    mounted() {
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera( 10, 1, 0.1, 1000 );
        const container = document.getElementById('atom-container');
        const renderer = new THREE.WebGLRenderer({ alpha: true });

        renderer.setSize( 300, 300 );
        container.appendChild( renderer.domElement );

        const sphereG = new THREE.SphereGeometry( 0.15, 32, 16 ); 
        const sphereM = new THREE.MeshBasicMaterial( { color: 0x000000 } ); 
        const sphere = new THREE.Mesh( sphereG, sphereM );
        scene.add(sphere);

        const createTorus = (position, rotation) => {
            const torusG = new THREE.TorusGeometry(2, 0.075, 16, 100);
            const torusM = new THREE.MeshBasicMaterial({ color: 0x000000 });
            const torus = new THREE.Mesh(torusG, torusM);
            torus.position.copy(position);
            torus.rotation.set(...rotation);
            scene.add(torus);
            return torus;
        };

        const torusMeshes = [
            createTorus(new THREE.Vector3(0, 0, 0), [2, 2.5, 0]),
            createTorus(new THREE.Vector3(0, 0, 0), [2, -2.5, 0])
        ];
        
        camera.position.z = 30;

        function animate() {
            requestAnimationFrame( animate );

            const orbitSpeed = 0.005;
            torusMeshes[0].rotation.x -= orbitSpeed;
            torusMeshes[1].rotation.x -= orbitSpeed;

            renderer.render( scene, camera );
        }

        animate();
    }
}
</script>
