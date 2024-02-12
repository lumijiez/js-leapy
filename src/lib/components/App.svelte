<script>
  import { Canvas } from '@threlte/core'
  import Scene from './Scene.svelte'
  // import { Pane, Button } from 'svelte-tweakpane-ui'
  // import { xrot1, yrot1, zrot1, xpos, ypos, zpos } from './stores';
</script>

<!-- <Pane
  title="Transitions"
  position="fixed"
>
  <Button
    title="XROT + {$xrot1}"
    on:click={() => {
      $xrot1 = $xrot1 + 0.1 
    }}
  />
  <Button
    title="YROT + {$yrot1}"
    on:click={() => {
      $yrot1 = $yrot1 + 0.1 
    }}
  />
  <Button
    title="ZROT + {$zrot1}"
    on:click={() => {
      $zrot1 = $zrot1 + 0.1 
    }}
  />

  <Button
    title="XROT - {$xrot1}"
    on:click={() => {
      $xrot1 = $xrot1 - 0.1 
    }}
  />
  <Button
    title="YROT - {$yrot1}"
    on:click={() => {
      $yrot1 = $yrot1 - 0.1 
    }}
  />
  <Button
    title="ZROT - {$zrot1}"
    on:click={() => {
      $zrot1 = $zrot1 - 0.1 
    }}
  />
</Pane>

<Pane
  title="Moves"
  position="fixed"
>
  <Button
    title="XPOS + {$xpos}"
    on:click={() => {
      $xpos = $xpos + 0.5
    }}
  />
  <Button
    title="YPOS + {$ypos}"
    on:click={() => {
      $ypos = $ypos + 0.5
    }}
  />
  <Button
    title="ZPOS + {$zpos}"
    on:click={() => {
      $zpos = $zpos + 0.5
    }}
  />

  <Button
    title="XPOS - {$xpos}"
    on:click={() => {
      $xpos = $xpos - 0.5
    }}
  />
  <Button
    title="YPOS - {$ypos}"
    on:click={() => {
      $ypos = $ypos - 0.5
    }}
  />
  <Button
    title="ZPOS - {$zpos}"
    on:click={() => {
      $zpos = $zpos - 0.5
    }}
  />
</Pane> -->
<!-- <div class="access-denied">
  Access denied! Come back when older!
</div> -->
<Canvas>
  <Scene />
</Canvas>

<style>

  .access-denied {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      background-color: #ff6961; /* Red color, you can change it */
      color: #fff; /* White text */
      text-align: center;
      font-size: xx-large;
      padding: 10px;
    }
</style>