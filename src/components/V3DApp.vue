<template>
  <div :id="containerId">
    <div class="fixed-bottom-right q-ma-md q-gutter-sm">
    <div >Cubes <span :class="Cube ? 'text-white bg-green' : 'text-white bg-red'">{{ Cube }}</span> Sphere <span :class="Sphere ? 'text-white bg-green' : 'text-white bg-red'">{{ Sphere }}</span></div>
    <q-btn
      color="grey"
      label="Cube"
      @click="Cube=true"
    />
        <q-btn
      color="blue-grey"
      label="Sphere"
      @click="Sphere=true"
    />
    </div>
    <q-dialog v-model="Cube">
      <q-card>
        <q-card-section>
          <div class="text-h6">This is a Cube</div>
        </q-card-section>
        <q-separator inset/>

        <q-card-section
          class="q-pt-none"
        >Lorem ipsum dolor sit amet consectetur adipisicing elit. Rerum repellendus sit voluptate voluptas eveniet porro. Rerum blanditiis perferendis totam, ea at omnis vel numquam exercitationem aut, natus minima, porro labore.</q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
        <q-dialog v-model="Sphere">
      <q-card>
        <q-card-section>
          <div class="text-h6">This is a Sphere</div>
        </q-card-section>

        <q-card-section
          class="q-pt-none"
        >Lorem ipsum dolor sit amet consectetur adipisicing elit. Rerum repellendus sit voluptate voluptas eveniet porro. Rerum blanditiis perferendis totam, ea at omnis vel numquam exercitationem aut, natus minima, porro labore.</q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import Vue from 'vue';

import * as V3DApp from "../v3dApp/app.js";
import * as LOGIC from "../v3dApp/visual_logic.js";

const eventBus = new Vue();

export default {
  name: "V3DApp",

  data() {
    return {
      containerId: V3DApp.CONTAINER_ID,
      Cube: false,
      Sphere: false,
    };
  },

  mounted: function () {
    v3d.PL = v3d.PL || {};
    v3d.PL._vueEventBus = eventBus;
    eventBus.$on('Object-clicked', data => {
      console.log('data: ', data.id);
      this[data.id] = true;
    });

    V3DApp.createApp('app.gltf');
    //V3DApp;
  },
};
</script>

<style>
@import "../v3dApp/app.css";
</style>
