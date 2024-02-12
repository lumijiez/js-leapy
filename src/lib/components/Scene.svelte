<script>
  import {T, useFrame, useRender, useThrelte} from '@threlte/core'
  import { ContactShadows, Float, Grid, OrbitControls, Text } from '@threlte/extras'
  import { onMount } from 'svelte';
  import Zero from './models/0.svelte'
  import One from './models/1.svelte'
  import Two from './models/2.svelte'
  import Three from './models/3.svelte'
  import Four from './models/4.svelte'
  import Five from './models/5.svelte'
  import Six from './models/6.svelte'
  import Seven from './models/7.svelte'
  import Eight from './models/8.svelte'
  import Nine from './models/9.svelte'
  import Colon from './models/colon.svelte'
  import Room from './models/shifthappens.svelte'
  import { xrot, yrot, zrot, xpos, ypos, zpos, xpos1, ypos1, zpos1, xrot1, yrot1, zrot1, goingForward } from './stores';
  import Dancer from './models/cute_cat_in_cute_banana.svelte'
  import Stars from './Stars.svelte'
  import { EffectComposer } from "three/examples/jsm/postprocessing/EffectComposer.js";
  import { RenderPass } from "three/examples/jsm/postprocessing/RenderPass.js";
  import { OutputPass } from "three/examples/jsm/postprocessing/OutputPass.js";
  import { UnrealBloomPass } from "three/examples/jsm/postprocessing/UnrealBloomPass.js";
  import {Color, LinearSRGBColorSpace, PCFSoftShadowMap, PMREMGenerator, SRGBColorSpace, Vector2} from "three";

  let day1, day2, hour1, hour2, minute1, minute2, second1, second2;

  let roomRef, kittyRef;

  const { scene, renderer, camera } = useThrelte();

  renderer.outputColorSpace = LinearSRGBColorSpace;
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = PCFSoftShadowMap;

  let pmrem = new PMREMGenerator(renderer);
  let envMapRT;

  const composer = new EffectComposer(renderer);
  composer.setSize(innerWidth, innerHeight);

  const setupEffectComposer = () => {
    const renderPass = new RenderPass(scene, camera.current);
    composer.addPass(renderPass);

    const bloomPass = new UnrealBloomPass(new Vector2(innerWidth, innerHeight), 0.275, 1, 0);
    composer.addPass(bloomPass);

    const outputPass = new OutputPass();
    composer.addPass(outputPass);
  }

  let scale = 7;
  let pos = 3;

  let first = [pos, 0, 0];
  let second = [pos * 2, 0, 0];
  let third = [pos * 3, 0, 0];
  let fourth = [pos * 4, 0, 0];
  let fifth = [pos * 5, 0, 0];
  let sixth = [pos * 6, 0, 0];
  let seventh = [pos * 7, 0, 0];
  let eighth = [pos * 8, 0, 0];
  let ninth = [pos * 9, 0, 0];
  let tenth = [pos * 10, 0, 0];
  let eleventh = [pos * 11, 0, 0];

  function timeUntilFeb29() {
    const targetDate = new Date('2024-02-29T00:00:00Z').getTime();
    const currentDate = new Date().getTime();

    const timeDifference = targetDate - currentDate;

  let days = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
  let hours = Math.floor((timeDifference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  let minutes = Math.floor((timeDifference % (1000 * 60 * 60)) / (1000 * 60));
  let seconds = Math.floor((timeDifference % (1000 * 60)) / 1000);

  days = String(days).length === 1 ? '0' + String(days) : String(days);
  hours = String(hours).length === 1 ? '0' + String(hours) : String(hours);
  minutes = String(minutes).length === 1 ? '0' + String(minutes) : String(minutes);
  seconds = String(seconds).length === 1 ? '0' + String(seconds) : String(seconds);

  day1 = String(days)[0];
  day2 = String(days)[1];
  hour1 = String(hours)[0];
  hour2 = String(hours)[1];
  minute1 = String(minutes)[0];
  minute2 = String(minutes)[1];
  second1 = String(seconds)[0];
  second2 = String(seconds)[1];
}

setInterval(() => timeUntilFeb29(), 1000);

onMount(() => {
  setupEffectComposer();
  timeUntilFeb29();
})

  useRender(({scene}) => {
    if ($xpos1 <= -33) {
      $yrot1 = 15.3;
      $xpos1 = -33;
      $ypos1 = 12.5;
      $zpos1 = 10;
      $goingForward = false;
    }
    if ($xpos1 >= -29) {
      $yrot1 = 18.4;
      $xpos1 = -29;
      $ypos1 = 13;
      $zpos1 = 0;
      $goingForward = true;
    }


    if ($goingForward) {
      $xpos1 -= (4 / 300);
      $ypos1 -= (0.5 / 300);
      $zpos1 += (10 / 300);
    } else {
      $xpos1 += (4 / 300);
      $ypos1 += (0.5 / 300);
      $zpos1 -= (10 / 300);
    }

    if (envMapRT) envMapRT.dispose();

    roomRef.visible = false;
    scene.background = null;
    envMapRT = pmrem.fromScene(scene, 0, 0.1, 1000);
    scene.background = new Color('#598889').multiplyScalar(0.05);
    roomRef.visible = true;

    roomRef.traverse((child) => {
      if (child?.material?.envMapIntensity) {
        child.material.envMap = envMapRT.texture;
        child.material.envMapIntensity = 50;
        child.material.normalScale.set(0.3, 0.3);
      }
    })

    composer.render();
  })


</script>

<T.PerspectiveCamera
  makeDefault
  position={[-145.38894411294393, 59.28506571053629, -73.89685946620432]}
  rotation={[-2.607235741023601, -0.9309163275599863, -2.6983806520336744]}
  fov={25}
  focus={10}
>
  <OrbitControls
    enableZoom={true}
    enableDamping
    target.x={5}
    target.y={10}
    target.z={20}
  />
</T.PerspectiveCamera>

<T.DirectionalLight
  intensity={3}
  position.x={5}
  position.y={10}
/>
<T.AmbientLight intensity={0.4} />

<Grid
  position.y={-0.001}
  cellColor="#ffffff"
  sectionColor="#ffffff"
  sectionThickness={0}
  fadeDistance={25}
  cellSize={2}
/>

<ContactShadows
  scale={10}
  blur={2}
  far={2.5}
  opacity={0.5}
/>

<Room castShadow receiveShadow scale={0.01} bind:ref={roomRef} />

<Dancer bind:ref={kittyRef} rotation={[$xrot1, $yrot1, $zrot1]} position={[$xpos1, $ypos1, $zpos1]} />

<T.Group rotation={[$xrot, $yrot, $zrot]} position={[$xpos, $ypos, $zpos]} >
  {#if day1 === '0'}
  <Zero position={first} scale={scale} />
{:else if day1 === '1'}
  <One position={first} scale={scale} />
{:else if day1 === '2'}
  <Two position={first} scale={scale} />
{:else if day1 === '3'}
  <Three position={first} scale={scale} />
{:else if day1 === '4'}
  <Four position={first} scale={scale} />
{:else if day1 === '5'}
  <Five position={first} scale={scale} />
{:else if day1 === '6'}
  <Six position={first} scale={scale} />
{:else if day1 === '7'}
  <Seven position={first} scale={scale} />
{:else if day1 === '8'}
  <Eight position={first} scale={scale} />
{:else if day1 === '9'}
  <Nine position={first} scale={scale} />
{/if}

{#if day2 === '0'}
  <Zero position={second} scale={scale} />
{:else if day2 === '1'}
  <One position={second} scale={scale} />
{:else if day2 === '2'}
  <Two position={second} scale={scale} />
{:else if day2 === '3'}
  <Three position={second} scale={scale} />
{:else if day2 === '4'}
  <Four position={second} scale={scale} />
{:else if day2 === '5'}
  <Five position={second} scale={scale} />
{:else if day2 === '6'}
  <Six position={second} scale={scale} />
{:else if day2 === '7'}
  <Seven position={second} scale={scale} />
{:else if day2 === '8'}
  <Eight position={second} scale={scale} />
{:else if day2 === '9'}
  <Nine position={second} scale={scale} />
{/if}

<Colon position={third} scale={scale} />

{#if hour1 === '0'}
  <Zero position={fourth} scale={scale} />
{:else if hour1 === '1'}
  <One position={fourth} scale={scale} />
{:else if hour1 === '2'}
  <Two position={fourth} scale={scale} />
{/if}

{#if hour2 === '0'}
  <Zero position={fifth} scale={scale} />
{:else if hour2 === '1'}
  <One position={fifth} scale={scale} />
{:else if hour2 === '2'}
  <Two position={fifth} scale={scale} />
{:else if hour2 === '3'}
  <Three position={fifth} scale={scale} />
{:else if hour2 === '4'}
  <Four position={fifth} scale={scale} />
{:else if hour2 === '5'}
  <Five position={fifth} scale={scale} />
{:else if hour2 === '6'}
  <Six position={fifth} scale={scale} />
{:else if hour2 === '7'}
  <Seven position={fifth} scale={scale} />
{:else if hour2 === '8'}
  <Eight position={fifth} scale={scale} />
{:else if hour2 === '9'}
  <Nine position={fifth} scale={scale} />
{/if}

<Colon position={sixth} scale={scale} />

{#if minute1 === '0'}
  <Zero position={seventh} scale={scale} />
{:else if minute1 === '1'}
  <One position={seventh} scale={scale} />
{:else if minute1 === '2'}
  <Two position={seventh} scale={scale} />
{:else if minute1 === '3'}
  <Three position={seventh} scale={scale} />
{:else if minute1 === '4'}
  <Four position={seventh} scale={scale} />
{:else if minute1 === '5'}
  <Five position={seventh} scale={scale} />
{:else if minute1 === '6'}
  <Six position={seventh} scale={scale} />
{/if}

{#if minute2 === '0'}
  <Zero position={eighth} scale={scale}  />
{:else if minute2 === '1'}
  <One position={eighth} scale={scale}  />
{:else if minute2 === '2'}
  <Two position={eighth} scale={scale}  />
{:else if minute2 === '3'}
  <Three position={eighth} scale={scale}  />
{:else if minute2 === '4'}
  <Four position={eighth} scale={scale}  />
{:else if minute2 === '5'}
  <Five position={eighth} scale={scale}  />
{:else if minute2 === '6'}
  <Six position={eighth} scale={scale}  />
  {:else if minute2 === '7'}
  <Seven position={eighth} scale={scale}  />
  {:else if minute2 === '8'}
  <Eight position={eighth} scale={scale}  />
  {:else if minute2 === '9'}
  <Nine position={eighth} scale={scale}  />
{/if}

<Colon position={ninth} scale={scale} />

{#if second1 === '0'}
  <Zero position={tenth} scale={scale} />
{:else if second1 === '1'}
  <One position={tenth} scale={scale} />
{:else if second1 === '2'}
  <Two position={tenth} scale={scale} />
  {:else if second1 === '3'}
  <Three position={tenth} scale={scale} />
  {:else if second1 === '4'}
  <Four position={tenth} scale={scale} />
  {:else if second1 === '5'}
  <Five position={tenth} scale={scale} />
  {:else if second1 === '6'}
  <Six position={tenth} scale={scale} />

{/if}

{#if second2 === '0'}
  <Zero position={eleventh} scale={scale} />
{:else if second2 === '1'}
  <One position={eleventh} scale={scale} />
{:else if second2 === '2'}
  <Two position={eleventh} scale={scale} />
{:else if second2 === '3'}
  <Three position={eleventh} scale={scale} />
{:else if second2 === '4'}
  <Four position={eleventh} scale={scale} />
{:else if second2 === '5'}
  <Five position={eleventh} scale={scale} />
{:else if second2 === '6'}
  <Six position={eleventh} scale={scale} />
  {:else if second2 === '7'}
  <Seven position={eleventh} scale={scale} />
  {:else if second2 === '8'}
  <Eight position={eleventh} scale={scale} />
  {:else if second2 === '9'}
  <Nine position={eleventh} scale={scale} />
{/if}


</T.Group>

<Stars />
