// @ts-nocheck
<script>
  import {T, useRender, useThrelte} from '@threlte/core'
  import { spring } from 'svelte/motion';
  import {ContactShadows, Float, OrbitControls} from '@threlte/extras'
  import { onMount } from 'svelte';
  import Stars from './Stars.svelte'
  import Cake from '../models/cake.svelte'
  import Two from '../models/2.svelte'
  import Zero from '../models/0.svelte'
  import Text from '../models/text.svelte'
  import Heart from '../models/love_low_poly.svelte'
  import { EffectComposer } from "three/examples/jsm/postprocessing/EffectComposer.js";
  import { RenderPass } from "three/examples/jsm/postprocessing/RenderPass.js";
  import { OutputPass } from "three/examples/jsm/postprocessing/OutputPass.js";
  import { UnrealBloomPass } from "three/examples/jsm/postprocessing/UnrealBloomPass.js";
  import {
    Color,
    LinearSRGBColorSpace,
    PCFSoftShadowMap,
    PMREMGenerator,
    Vector2
  } from "three";
  import {degToRad} from "three/src/math/MathUtils.js";
  import {posY, rotValX, rotValY, rotValZ, LposX} from "$lib/components/actual/stores.js";

  const { scene, renderer, camera } = useThrelte();

  renderer.outputColorSpace = LinearSRGBColorSpace;
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = PCFSoftShadowMap;

  let cakeRef;
  let zeroRef;
  let twoRef;
  let textRef;
  let hearts = Array.of(10);
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

  let direction = 1;
  let scale = spring(1);
  let posZ = -300;
  let audioContext;
  let analyser;
  let audioElement;
  let scale_bands = [];

  const setupAudio = () => {
      audioContext = new (window.AudioContext)();
      analyser = audioContext.createAnalyser();
      analyser.fftSize = 32;

      const source = audioContext.createMediaElementSource(audioElement);
      source.connect(analyser);
      analyser.connect(audioContext.destination);

      const bufferLength = analyser.frequencyBinCount;
      const numBands = 10;
      const bandSize = Math.floor(bufferLength / numBands);

      const dataArray = new Uint8Array(bufferLength);

      const updateVolume = () => {
          analyser.getByteFrequencyData(dataArray);

          scale_bands = [];
          for (let i = 0; i < numBands; i++) {
              let sum = 0;
              for (let j = 0; j < bandSize; j++) {
                  sum += dataArray[i * bandSize + j];
              }
              let average = sum / bandSize;
              scale_bands.push(average);
          }

          scale.set(scale_bands.reduce((acc, val) => acc + val, 0) / numBands / 100);
      };

      const updateLoop = () => {
          updateVolume();
          requestAnimationFrame(updateLoop);
      };

      updateLoop();
  };

onMount(() => {
    setupAudio();
    setupEffectComposer();
    audioElement.play()
})

  useRender(({scene}) => {
    if (envMapRT) envMapRT.dispose();

    cakeRef.visible = false;
    zeroRef.visible = false;
    twoRef.visible = false;
    textRef.visible = false;
    hearts.forEach(heart => {
        heart.visible = false;
    })
    scene.background = null;
    envMapRT = pmrem.fromScene(scene, 0, 0.1, 1000);
    scene.background = new Color('pink').multiplyScalar(0.05);
    cakeRef.visible = true;
    twoRef.visible = true;
    zeroRef.visible = true;
    textRef.visible = true;
      hearts.forEach(heart => {
          heart.visible = true;
      })

    cakeRef.traverse((child) => {
      if (child?.material?.envMapIntensity) {
        child.material.envMap = envMapRT.texture;
        child.material.envMapIntensity = 50;
        child.material.normalScale.set(0.3, 0.3);
      }
    })

    twoRef.traverse((child) => {
      if (child?.material?.envMapIntensity) {
        child.material.envMap = envMapRT.texture;
        child.material.envMapIntensity = 10;
        child.material.normalScale.set(0.3, 0.3);
      }
    })

    zeroRef.traverse((child) => {
      if (child?.material?.envMapIntensity) {
        child.material.envMap = envMapRT.texture;
        child.material.envMapIntensity = 10;
        child.material.normalScale.set(0.3, 0.3);
      }
    })

    textRef.traverse((child) => {
      if (child?.material?.envMapIntensity) {
        child.material.envMap = envMapRT.texture;
        child.material.envMapIntensity = 30;
        child.material.normalScale.set(0.3, 0.3);
      }
    })

      hearts.forEach(heart => {
          heart.traverse((child) => {
              if (child?.material?.envMapIntensity) {
                  child.material.envMap = envMapRT.texture;
                  child.material.envMapIntensity = 100;
                  child.material.normalScale.set(0.7, 0.7);
              }
          })
      })



    if (posZ < -300) direction *= -1
    else if (posZ > 300) direction *= -1;

    posZ = posZ + direction;
    console.log(posZ)
    composer.render();
  })


</script>
<audio src="https://www.chosic.com/wp-content/uploads/2022/01/Missing-You.mp3" bind:this={audioElement}></audio>

<T.PerspectiveCamera
  makeDefault
  position={[-350, 250, 0]}
  rotation={[0, 0, 0]}
  fov={25}
  focus={10}
>
  <OrbitControls
    enableZoom={true}
    enableDamping
    target.x={20}
    target.y={50}
    target.z={0}
  />
</T.PerspectiveCamera>

<T.DirectionalLight
  intensity={1}
  target={textRef}
  position.x={$LposX}
  position.y={$posY}
  position.z={posZ}
/>
<T.AmbientLight intensity={0.4} />

<ContactShadows
  scale={10}
  blur={2}
  far={2.5}
  opacity={0.5}
/>

<Stars />

<Float
        floatIntensity={1}
        rotationIntensity={0.4}
        rotationSpeed={[2, 0, 0]}
>
<Text bind:ref={textRef} scale={30}  position={[100, 45, -120]} rotation={[degToRad($rotValX), degToRad($rotValY), degToRad($rotValZ)]} />
</Float>

<Float floatIntensity={1} floatingRange={[1, 2]} speed={1}>
<T.Group>
<Cake bind:ref={cakeRef} rotation={[0, degToRad(30), 0]} scale={0.5} />
  <Two bind:ref={twoRef} position={[-19, 60, -15]} rotation={[0, degToRad(-90), 0]}  scale={30}/>
  <Zero bind:ref={zeroRef} position={[-19, 60, -5]} rotation={[0, degToRad(-90), 0]}  scale={30}/>
</T.Group>
</Float>

<Float
       rotationIntensity={[0.2, 0.5, 0.5]}
       rotationSpeed={[1, 1, 1]}>
    <Heart scale={5} bind:ref={hearts[0]} position={[140, -50, -120]} rotation={[0, degToRad(-90), 0]}/>
</Float>

<Float
        rotationIntensity={[1, 1, 1]}
        rotationSpeed={[1, 1, 1]}>
    <Heart scale={5} bind:ref={hearts[1]} position={[40, -70, -170]} rotation={[0, degToRad(-90), 0]}/>
</Float>

<Float
        rotationIntensity={[0.2, 0.5, 0.5]}
        rotationSpeed={[1, 1, 1]}>
    <Heart scale={5} bind:ref={hearts[1]} position={[90, 40, -170]} rotation={[0, degToRad(-90), 0]}/>
</Float>

<Float
        rotationIntensity={1}
        rotationSpeed={[1, 1, 1]}>
    <Heart scale={5} bind:ref={hearts[1]} position={[60, -70, 170]} rotation={[0, degToRad(-90), 0]}/>
</Float>

<Float
        rotationIntensity={1}
        rotationSpeed={[1, 1, 1]}>
    <Heart scale={5} bind:ref={hearts[1]} position={[100, 70, 170]} rotation={[0, degToRad(-90), 0]}/>
</Float>

<Float
        rotationIntensity={1}
        rotationSpeed={[1, 1, 1]}>
    <Heart scale={5} bind:ref={hearts[1]} position={[80, -10, 120]} rotation={[0, degToRad(-90), 0]}/>
</Float>



