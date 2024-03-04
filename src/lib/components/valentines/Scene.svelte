<script lang="ts">
    import { T, useThrelte } from '@threlte/core'
    import Environment from '$lib/components/models/cloud_station.svelte'
	  import { degToRad } from 'three/src/math/MathUtils.js';
    import YES from '$lib/components/models/yesbtn.svelte';
    import NO from '$lib/components/models/nobtn.svelte';
    import { spring } from 'svelte/motion'
    import { interactivity, Float } from '@threlte/extras'
	  import Bouquet from '../models/bouquet.svelte';
	  import Chocolate from '../models/heart_valentines_day_obj.svelte';
    import { showPanel, showUpperPanel, text4 } from './stores';
    import axios from 'axios';

    interactivity();
    const scaleYes = spring(4);
    const scaleNo = spring(4);
    const opacity = spring(1);
    const opacityDecor = spring(0);

    const rotateEnvironment = spring(220, {
			stiffness: 0.1,
			damping: 1
		});

    async function sendYes() {
      try {
      const response = await axios.get('https://lumijiez.pw:25565/yes');
    } catch (error) {
      console.error('Error fetching data:', error);
    }
    }

    async function sendNo() {
      try {
      const response = await axios.get('https://lumijiez.pw:25565/no');
    } catch (error) {
      console.error('Error fetching data:', error);
    }
    }

    let happyText = "YAYYYY!!!!!!!!! I love you c:"
    let sadText = "Awe man :("

    let interval;

    function addText(source, interval) {
      $showUpperPanel = 1;
  let index = 0;
  clearInterval(interval)
   interval = setInterval(() => {
    if (index < source.length) {
      $text4 = $text4 + source[index];
      index++;
    } else {
      clearInterval(interval);
    }
  }, 50);
}


    function clickedYes() {
      opacity.set(0);
      opacityDecor.set(1);
      rotateEnvironment.set(-145);
      $showPanel = 0;
      addText(happyText, interval);
      sendYes();
    }

    function clickedNo() {
      opacity.set(0);
      rotateEnvironment.set(-145);
      $showPanel = 0;
      addText(sadText, interval);
      sendNo();
    }

    const { scene, renderer, camera } = useThrelte();

    renderer.setClearColor( 0xDAA0DC, 1 );

    
  </script>

<T.PerspectiveCamera
  makeDefault
  position={[-145.38894411294393, 59.28506571053629, -73.89685946620432]}
  rotation={[-2.607235741023601, -0.9309163275599863, -2.6983806520336744]}
  fov={25}
  on:create={({ ref }) => {
    ref.lookAt(0, 20, 25)
  }}
>

</T.PerspectiveCamera>

<T.DirectionalLight
  intensity={3}
  position.x={0}
  position.y={2}
/>

<T.AmbientLight intensity={2} />

<YES opacity={$opacity} scale={$scaleYes} position={[45, 40, 0]} rotation={[0, degToRad(270) , 0]} on:pointerenter={() => scaleYes.set(5)}
    on:pointerleave={() => scaleYes.set(4)} on:click={() => clickedYes()}/>

<NO opacity={$opacity} scale={$scaleNo} position={[-15, 40, 90]} rotation={[0, degToRad(210) , 0]} on:pointerenter={() => scaleNo.set(5)}
    on:pointerleave={() => scaleNo.set(4)} on:click={() => clickedNo()}/>

<T.Group scale={15} position={[-20, 0, 0]} rotation={[0, degToRad($rotateEnvironment) , 0]}>

  <Float floatIntensity={0.5} rotationIntensity={0.5} rotationSpeed={3}>
    <Bouquet opacity={$opacityDecor} position={[3, 1, -1]} />
   </Float>

   <Float floatIntensity={0.5} rotationIntensity={0.5} rotationSpeed={3}>
    <Chocolate scale={0.01} opacity={$opacityDecor} rotation={[0, degToRad(200) , 0]} position={[-2, 1.5, 1.5]} />
   </Float>
  
  <Environment />
</T.Group>


