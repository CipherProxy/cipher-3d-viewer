<template>
  <div class="main">
    
  </div>
</template>

<script>

import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { VRMLoader } from 'three/examples/jsm/loaders/VRMLoader';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

// There is code for two viewers included in this program (GLB/VRM)
// Only one can be enabled at a time - Used notes to disable unused code for now.

// This is the VRM loader

// let container, controls;
// let camera, scene, renderer, light;

/*
init();
animate();

function init() {
	// Instantiating the three.js scene
	container = document.createElement('div');
	document.body.appendChild(container);
	camera = new THREE.PerspectiveCamera(90, window.innerWidth / window.innerHeight, 0.01, 20);
	camera.position.set(0,2.5,-2);
	scene = new THREE.Scene();
	light = new THREE.HemisphereLight(0xbbbbff, 0x444422);
	light.position.set(0, 1, 0);
	scene.add(light);
	// Creating the loader
	const loader = new VRMLoader();
	loader.load('https://cnoib-2yaaa-aaaaf-qadja-cai.raw.ic0.app/nft/54', function(vrm) {
		scene.add(vrm.scene);
		console.log(scene);
	});
	renderer = new THREE.WebGLRenderer({antialias: true, alpha: true});
	renderer.setClearColor( 0xffffff, 0 );
	renderer.setPixelRatio(window.devicePixelRatio);
	renderer.setSize(window.innerWidth, window.innerHeight);
	renderer.outputEncoding = THREE.sRGBEncoding;
	container.appendChild(renderer.domElement);
	// Instantiate Controllers
	controls = new OrbitControls(camera, renderer.domElement);
	controls.enableDamping = true;
	controls.target.set(0, 0.9, 0);
	controls.update();
	// Function for auto-resize
	window.addEventListener('resize', onWindowResize);
}
// The actual function for window resize
function onWindowResize() {
	camera.aspect = window.innerWidth / window.innerHeight;
	camera.updateProjectionMatrix();
	renderer.setSize(window.innerWidth, window.innerHeight);
}
// Animation function (Temporarily Removed)
function animate() {
	requestAnimationFrame(animate);
	controls.update(); // To support "damping"
	renderer.render(scene, camera);
}
*/

// This is the GLTF/GLB viewer
// ########################################################################

let camera, scene, renderer;

init();
render();

function init() {       
	const container = document.createElement( 'div' );
  	container.setAttribute("class", "content");
	document.body.appendChild( container );

	camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 0.25, 20 );
	camera.position.set( -5, 1.5, 5 );

	scene = new THREE.Scene();

	let params = (new URL(document.location)).searchParams;
    let tokenIndex = params.get('token');

	const loader = new GLTFLoader();
	loader.load( 'https://piwdi-uyaaa-aaaam-qaojq-cai.raw.ic0.app/assets/' + tokenIndex + '_model.glb', function ( gltf ) {
    gltf.scene.traverse( function ( child ) {
      if ( child.isMesh ) {
              
      }
    });
  	scene.add( gltf.scene );
  	console.log("GLTF NFT loaded successfully!");
  	render();
  	});
        
	renderer = new THREE.WebGLRenderer( { antialias: true, alpha: true } );
	renderer.setPixelRatio( window.devicePixelRatio );
	renderer.setSize( window.innerWidth, window.innerHeight );
	renderer.toneMapping = THREE.ACESFilmicToneMapping;
	renderer.toneMappingExposure = 1;
	renderer.outputEncoding = THREE.sRGBEncoding;
  	renderer.setClearColor( 0xffffff, 0 );
	container.appendChild( renderer.domElement );

	const controls = new OrbitControls( camera, renderer.domElement );
	controls.addEventListener( 'change', render ); // use if there is no animation loop
	controls.minDistance = 2;
	controls.maxDistance = 10;
	controls.target.set( 0, 0, - 0.2 );
	controls.update();

  const ambientLight = new THREE.AmbientLight( 0xffcf40, 1 );
  scene.add(ambientLight);
  const pointLight = new THREE.PointLight( 0xffffff, 3 );
  pointLight.position.set(1,5,1);
  scene.add( pointLight );
  console.log(scene);
  console.log(scene.children[0].color);

	window.addEventListener( 'resize', onWindowResize );

}
function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight;
	camera.updateProjectionMatrix();
  renderer.setSize( window.innerWidth, window.innerHeight );
  render();
}

function render() {
  renderer.render( scene, camera );
}

// ##########################################################

export default {
  name: "App",
  components: {
    
  },
}
</script>

<style>
html {
  background-color: black;
  /* background-image: url('spaceBackground.jpg'); */
  background-size: cover;
  background-repeat: no-repeat;
  height: 100%;
}
body{  
  overflow: hidden;
}
.content {
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: 100%;
  overflow: hidden;
}

</style>
