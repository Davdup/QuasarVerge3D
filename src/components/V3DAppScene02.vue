<template>
  <div :id="containerId">
    <div class="fixed-top-right q-mt-xl q-gutter-sm">
      <div class="q-pa-md" style="width: 300px">
        <q-list padding bordered class="rounded-borders bg-blue text-white">
          <q-expansion-item
            dense
            dense-toggle
            expand-separator
            icon="play_circle_outline"
            label="Animated Modules"
            header-class="bg-blue-9 text-white"
            expand-icon-class="text-white"
          >
            <q-card class="text-black bg-blue-2">
              <q-card-section>
                <div class="row justify-center" v-for="(anim, index) in anims" :key="index">
                  <div class="col q-pa-sm">{{ anim.name }}</div>
                  <div class="col">
                    <q-btn
                      :id="anim.btnId"
                      class="q-ma-xs"
                      :color="anim.completed ? 'text-white bg-green' : 'text-white bg-red'"
                      :label="anim.completed ? 'Open' : 'Close'"
                      @click="anim.completed=!anim.completed"
                    />
                  </div>
                </div>
              </q-card-section>
            </q-card>
          </q-expansion-item>
          <q-separator />
          <q-expansion-item
            dense
            dense-toggle
            expand-separator
            icon="info_outline"
            label="Info Overlays"
            header-class="bg-blue-9 text-white"
            expand-icon-class="text-white"
          >
            <q-card class="text-black bg-blue-2">
              <q-card-section>
                <div class="row justify-center" v-for="(info, index) in infos" :key="index">
                  <div class="col q-pa-sm">{{ info.name }}</div>
                  <div class="col">
                    <q-btn
                      :id="info.btnId"
                      class="q-ma-xs"
                      color="grey"
                      icon="info_outline"
                      @click="showModal(info.modalId)"
                    />
                  </div>
                </div>
                <!-- <div>
                  Cubes
                  <span
                    :class="infoModals.cubeModal ? 'text-white bg-green' : 'text-white bg-red'"
                  >{{ infoModals.cubeModal }}</span> Sphere
                  <span
                    :class="infoModals.sphereModal ? 'text-white bg-green' : 'text-white bg-red'"
                  >{{ infoModals.sphereModal }}</span>
                </div>-->
              </q-card-section>
            </q-card>
          </q-expansion-item>
        </q-list>
      </div>
    </div>

    <!-- dialog box for Cube -->
    <q-dialog v-model="infoModals.Sphere01">
      <q-card>
        <q-card-section>
          <div class="text-h6">This is a Big Sphere</div>
        </q-card-section>
        <q-separator inset />

        <q-card-section
          class="q-pt-none"
        >Lorem ipsum dolor sit amet consectetur adipisicing elit. Rerum repellendus sit voluptate voluptas eveniet porro. Rerum blanditiis perferendis totam, ea at omnis vel numquam exercitationem aut, natus minima, porro labore.</q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <!-- dialog box for Sphere -->
    <q-dialog v-model="infoModals.Sphere02">
      <q-card>
        <q-card-section>
          <div class="text-h6">This is a Small Sphere</div>
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
import Vue from "vue";

import * as V3DApp from "../v3dApp/app.js";
import * as LOGIC from "../../public/v3dApp/scene02_logic.js";

const eventBus = new Vue();

export default {
  name: "V3DApp",

  data() {
    return {
      containerId: V3DApp.CONTAINER_ID,
      infoModals: {
        Sphere01: false,
        Sphere02: false,
      },
      anims: [
        {
          name: "Big Sphere Module",
          btnId: "btnAnimCube",
          completed: false,
        },
        {
          name: "Small Sphere Module",
          btnId: "btnAnimSphere",
          completed: false,
        },
        // {
        //   name: "Other Module",
        //   btnId: "btnAnimOther",
        //   completed: false,
        // },
      ],
      infos: [
        {
          name: "Big Sphere info",
          btnId: "btnCube",
          modalId: "Sphere01",
        },
        {
          name: "Small Sphere info",
          btnId: "btnSphere",
          modalId: "Sphere02",
        },
      ],
    };
  },

  mounted: function () {
    v3d.PL = v3d.PL || {};
    v3d.PL._vueEventBus = eventBus;
    eventBus.$on("Object-clicked", (data) => {
      this.infoModals[data.id] = true;
    });
    eventBus.$on("showNotif", (data) => {
      this.showNotif(data.id);
    });

    V3DApp.createApp("scene02.gltf", "scene02_logic.js");
    //V3DApp;
  },

  methods: {
    showNotif(hoveredObject) {
      this.$q.notify({
        message: "Animate " + hoveredObject,
        timeout: "3000",
        progress: true,
        position: "top",
        color: "green",
        avatar: "https://cdn.quasar.dev/img/boy-avatar.png",
        actions: [
          {
            label: "Animate",
            color: "white",
            handler: () => {
              /* ... */
            },
          },
          {
            label: "Close",
            color: "red",
            handler: () => {
              /* ... */
            },
          },
        ],
      });
    },
    showModal(modalId) {
      this.infoModals[modalId] = true;
    },
  },
};
</script>

<style>
@import "../v3dApp/app.css";
</style>
