<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Baja Trophy Truck</title>
<style>
body { margin: 0; overflow: hidden; background: #cfa96b; }
#ui {
 position: absolute;
 top: 10px;
 left: 10px;
 color: white;
 font-family: Arial;
 font-weight: bold;
}
</style>
</head>
<body>
<div id="ui">⬅ ➡ steer | SPACE = nitro</div>

<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>

<script>
const scene = new THREE.Scene();
scene.background = new THREE.Color(0xe6c07b);

const camera = new THREE.PerspectiveCamera(75, innerWidth/innerHeight, 0.1, 1000);
camera.position.set(0, 6, 12);

const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(innerWidth, innerHeight);
document.body.appendChild(renderer.domElement);

scene.add(new THREE.HemisphereLight(0xffffff, 0x444444, 1.2));
const sun = new THREE.DirectionalLight(0xffffff, 1);
sun.position.set(10, 20, 10);
scene.add(sun);

const ground = new THREE.Mesh(
 new THREE.PlaneGeometry(500, 500, 200, 200),
 new THREE.MeshStandardMaterial({ color: 0xd9a441 })
);
ground.rotation.x = -Math.PI / 2;
scene.add(ground);

const truck = new THREE.Group();
const body = new THREE.Mesh(
 new THREE.BoxGeometry(3, 1, 6),
 new THREE.MeshStandardMaterial({ color: 0xd62828 })
);
body.position.y = 1.5;
truck.add(body);

const wheels=[];
for (let x of [-1.5,1.5]) {
 for (let z of [-2,2]) {
  const w=new THREE.Mesh(
   new THREE.CylinderGeometry(0.6,0.6,0.5,16),
   new THREE.MeshStandardMaterial({ color: 0x111 })
  );
  w.rotation.z=Math.PI/2;
  w.position.set(x,0.7,z);
  wheels.push(w);
  truck.add(w);
 }
}
scene.add(truck);

let left=false,right=false,nitro=false;
onkeydown=e=>{
 if(e.key==="ArrowLeft") left=true;
 if(e.key==="ArrowRight") right=true;
 if(e.key===" ") nitro=true;
}
onkeyup=e=>{
 if(e.key==="ArrowLeft") left=false;
 if(e.key==="ArrowRight") right=false;
 if(e.key===" ") nitro=false;
}

let speed=0.25,time=0;
function animate(){
 requestAnimationFrame(animate);
 time+=0.05;
 if(left) truck.rotation.y+=0.04;
 if(right) truck.rotation.y-=0.04;
 truck.translateZ(-(n
