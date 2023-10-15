<script lang="ts">
    import { T } from '@threlte/core'
    import { GLTF, OrbitControls } from '@threlte/extras'
     export let autoRotate = false;
  </script>
  
  <T.PerspectiveCamera
    makeDefault
    position={[450, 80, -50]}
    fov={25}
  >
    <OrbitControls
      {autoRotate}
      enableDamping
    />
  </T.PerspectiveCamera>

  <T.AmbientLight/>
  <T.DirectionalLight position={[1, 14,40]} intensity={0.1}/>

  
  <GLTF url="src/lib/assets/models/pond/scene.gltf" />
  