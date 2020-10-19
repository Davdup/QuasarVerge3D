<template>
  <div :id="containerId">
    <q-btn
      color="white"
      text-color="black"
      label="Dialog"
      class="fixed-bottom-right q-ma-md"
      @click="alert=true"
    />
    <q-dialog v-model="alert">
      <q-card>
        <q-card-section>
          <div class="text-h6">Alert</div>
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
      alert: LOGIC.alert2,
      test: LOGIC.testVar,
    };
  },

  mounted: function () {
    v3d.PL = v3d.PL || {};
    v3d.PL._vueEventBus = eventBus;

    V3DApp.createApp('app.gltf');
    //V3DApp;
    console.log("test :", this.test);
    console.log("alert :", alert);
  },
};
</script>

<style>
@import "../v3dApp/app.css";
</style>
