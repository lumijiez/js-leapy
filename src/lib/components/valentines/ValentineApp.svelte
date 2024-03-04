<script lang="ts">
    import { Canvas } from '@threlte/core'
    import Scene from './Scene.svelte'
    import { showPanel, showUpperPanel, text, text2, text3, text4 } from './stores';

    let neededText = "❤️Hello pretty girl Zani!❤️"
    let secondLine = "❤️Would you like to be my valentine this year?❤️"
    let thirdLine = "(You can click one of the buttons above)"

    function addText(source) {
  let index = 0;
  const interval = setInterval(() => {
    if (index < source.length) {
      $text = $text + source[index];
      index++;
    } else {
      addText2(secondLine)
      clearInterval(interval);
    }
  }, 50);
  
}

function addText2(source) {
  let index = 0;
  const interval = setInterval(() => {
    if (index < source.length) {
      $text2 = $text2 + source[index];
      index++;
    } else {
      addText3(thirdLine)
      clearInterval(interval);
    }
  }, 50);
}

function addText3(source) {
  let index = 0;
  const interval = setInterval(() => {
    if (index < source.length) {
      $text3 = $text3 + source[index];
      index++;
    } else {
      clearInterval(interval);
    }
  }, 50);
}

addText(neededText);

  </script>
  
  {#if $showUpperPanel}
  <div class="upper-text">
    {$text4}
  </div>
  {/if}


  {#if $showPanel}
  <div class="overlay">
    <p>{$text}</p>
    <p>{$text2}</p>
    <p>{$text3}</p>
  </div>
  {/if}

  <Canvas>
    <Scene />
  </Canvas>

  <style>
    .overlay {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: rgba(0, 0, 0, 0.3);
      color: white;
      padding: 20px;
      border-radius: 10px;
      font-size: xx-large;
      font-weight: 500;
      font-family: "Protest Strike", sans-serif;
      color: #ffa1cd
    }

    .upper-text {
    position: fixed;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    font-family: 'Roboto', sans-serif;
    font-weight: 700; /* Use 400 for regular weight */
    font-size: 60px; /* Adjust the font size as needed */
    background-color: rgba(0, 0, 0, 0.3);
    padding: 20px;
      border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Optional: Add a shadow effect */
    z-index: 999; /* Ensure it's above other content */
    color: #ffa1cd
  }

  </style>