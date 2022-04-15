<template>
  <div class="full-width full-height">
    <Renderer
      ref="renderer"
      antialias
      :orbit-ctrl="{ enableDamping: true, target }"
      resize="window"
      shadow
    >
      <Camera :position="{ x: 100, y: 200, z: 300 }" />
      <Scene ref="scene" background="#a0a0a0">
        <HemisphereLight />
        <AmbientLight />
        <PointLight
          :position="{ x: 100, y: 100, z: 100 }"
          intensity="2"
          color="rgba(255,0,0,1)"
        />
        <PointLight
          :position="{ x: -100, y: -100, z: 100 }"
          intensity="2"
          color="rgba(0,250,0,1)"
        />
        <DirectionalLight
          :position="{ x: 0, y: 200, z: 100 }"
          cast-shadow
          :shadow-camera="{ top: 180, bottom: -120, left: -120, right: 120 }"
          :intensity="0.5"
        />

        <!-- <Plane
          :width="2000"
          :height="2000"
          :rotation="{ x: -Math.PI / 2 }"
          receive-shadow
        >
          <PhongMaterial color="#999999" :props="{ depthWrite: false }" />
        </Plane> -->

        <FbxModel src="static/dancing.fbx" @load="onLoad" />
      </Scene>
    </Renderer>
  </div>
</template>

<script>
// Model from mixamo.com
import { AnimationMixer, Clock, Fog, GridHelper, Vector3 } from "three";
import {
  AmbientLight,
  PointLight,
  Camera,
  DirectionalLight,
  FbxModel,
  HemisphereLight,
  Renderer,
  Scene,
} from "troisjs";
export default {
  components: {
    AmbientLight,
    PointLight,
    Camera,
    DirectionalLight,
    FbxModel,
    HemisphereLight,
    Renderer,
    Scene,
  },
  data() {
    return {
      target: new Vector3(0, 100, 0),
    };
  },
  mounted() {
    const scene = this.$refs.scene.scene;
    scene.fog = new Fog("red", 200, 1000);
    const grid = new GridHelper(1000, 500, "red", "cyan");
    grid.material.opacity = 1;
    grid.material.transparent = false;
    this.$refs.scene.add(grid);
  },
  methods: {
    onLoad(object) {
      this.mixer = new AnimationMixer(object);
      const action = this.mixer.clipAction(object.animations[0]);
      action.play();
      object.traverse(function (child) {
        if (child.isMesh) {
          child.castShadow = true;
          child.receiveShadow = true;
        }
      });
      this.clock = new Clock();
      this.$refs.renderer.onBeforeRender(this.updateMixer);
    },
    updateMixer() {
      this.mixer.update(this.clock.getDelta());
    },
  },
};
</script>
