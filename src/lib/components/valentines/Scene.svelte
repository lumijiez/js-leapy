<script lang="ts">
    import { Canvas, T } from '@threlte/core'
    import { SheetObject, Theatre } from '@threlte/theatre'
  </script>
  <Canvas>
    <Theatre>
      <SheetObject
        key="Camera"
        let:Transform
      >
        <Transform>
          <T.PerspectiveCamera
            makeDefault
            position={[5, 10, 3]}
          />
        </Transform>
      </SheetObject>
      <SheetObject
        key="Cube"
        let:Transform
      >
        <Transform>
          <T.Mesh position.y={0.5}>
            <T.BoxGeometry />
            <T.MeshBasicMaterial color="hotpink" />
          </T.Mesh>
        </Transform>
      </SheetObject>
      <T.GridHelper />
    </Theatre>
  </Canvas>