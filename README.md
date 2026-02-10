<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Baja Trophy Truck</title>
<style>
body { margin: 0; overflow: hidden; background: #cfa96b; }
#ui { position: absolute; top: 10px; left: 10px; color: white; font-family: Arial; font-weight: bold; z-index:10;}
</style>
</head>
<body>
<div id="ui">⬅ ➡ steer | SPACE = nitro</div>
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
<script>
const scene = new THREE.Scene();
scene.background = new THREE.Color(0xe6c07b);

const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
camera.position.set(0, 5, 12);

const renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Lights
const hemi = new THREE.HemisphereLight(0xffffff, 0x444444, 1.2);
scene.add(hemi);
const sun = new THREE.DirectionalLight(0xffffff, 1);
sun.position.set(10, 20, 10);
scene.add(sun);

// Ground
const groundGeo = new THREE.PlaneGeometry(500, 500, 50, 50);
const groundMat = new THREE.MeshStandardMaterial({color:0xd9a441});
const ground = new THREE.Mesh(groundGeo, groundMat);
ground.rotation.x = -Math.PI/2;
scene.add(ground);

// Add a simple wave for bumps
groundGeo.vertices?.forEach(v=>v.z=Math.sin(v.x*0.05)*0.5);

// Truck
const truck = new THREE.Group();
const body = new THREE.Mesh(new THREE.BoxGeometry(3,1,6), new THREE.MeshStandardMaterial({color:0xd62828}));
body.position.y = 1.5;
truck.add(body);

// Wheels
const wheels=[];
for(let x of [-1.2,1.2]){
 for(let z of [-2,2]){
  const w = new THREE.Mesh(new THREE.CylinderGeometry(0.6,0.6,0.5,16), new THREE.MeshStandardMaterial({color:0x111111}));
  w.rotation.z = Math.PI/2;
  w.position.set(x,0.7,z);
  wheels.push(w);
  truck.add(w);
 }
}
scene.add(truck);

// Controls
let left=false,right=false,nitro=false;
document.addEventListener('keydown',e=>{
 if(e.key==='ArrowLeft') left=true;
 if(e.key==='ArrowRight') right=true;
 if(e.key===' ') nitro=true;
});
document.addEventListener('keyup',e=>{
 if(e.key==='ArrowLeft') left=false;
 if(e.key==='ArrowRight') right=false;
 if(e.key===' ') nitro=false;
});

let speed = 0.2;
let time = 0;

function animate(){
 requestAnimationFrame(animate);
 time+=0.05;

 if(left) truck.rotation.y += 0.05;
 if(right) truck.rotation.y -= 0.05;

 // Move truck forward
 truck.translateZ(-(nitro?0.6:0.2));

 // Wheels bounce
 wheels.forEach((w,i)=>w.position.y=0.7+Math.sin(time+i)*0.2);

 // Camera follows
 camera.position.lerp(new THREE.Vector3(truck.position.x, truck.position.y+5, truck.position.z+12),0.1);
 camera.lookAt(truck.position);

 renderer.render(scene,camera);
}
animate();

window.addEventListener('resize',()=>{
 camera.aspect=window.innerWidth/window.innerHeight;
 camera.updateProjectionMatrix();
 renderer.setSize(window.innerWidth,window.innerHeight);
});
</script>
</body>
</html>
