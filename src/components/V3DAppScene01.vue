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
    <q-dialog v-model="infoModals.Cube">
      <q-card style="width: 700px; max-width: 80vw;">
        <q-card-section>
          <div class="text-h6">This is a Cube</div>
        </q-card-section>

        <q-separator />

        <q-card-section class="q-pt-none scroll" style="max-height: 50vh">
          <div class="q-pa-md q-gutter-sm">
            <q-input ref="filter" filled v-model="filter" label="Filter">
              <template v-slot:append>
                <q-icon
                  v-if="filter !== ''"
                  name="clear"
                  class="cursor-pointer"
                  @click="resetFilter"
                />
              </template>
            </q-input>
            <q-tree :nodes="simple" node-key="label" :filter="filter" default-expand-all />
          </div>
        </q-card-section>

        <q-separator />

        <q-card-actions align="right">
          <q-btn flat label="Close" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <!-- dialog box for Sphere -->
    <q-dialog v-model="infoModals.Sphere">
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
import Vue from "vue";

import * as V3DApp from "../v3dApp/app.js";
import * as LOGIC from "../../public/v3dApp/scene01_logic.js";

const eventBus = new Vue();

export default {
  name: "V3DApp",

  data() {
    return {
      containerId: V3DApp.CONTAINER_ID,
      infoModals: {
        Cube: false,
        Sphere: false,
      },
      anims: [
        {
          name: "Cube Module",
          btnId: "btnAnimCube",
          completed: false,
        },
        {
          name: "Sphere Module",
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
          name: "Cube Module info",
          btnId: "btnCube",
          modalId: "Cube",
        },
        {
          name: "Sphere Module info",
          btnId: "btnSphere",
          modalId: "Sphere",
        },
      ],
      filter: "",
      simple: [
        {
          label: "Satisfied customers",
          children: [
            {
              label: "Good food",
              children: [
                { label: "Quality ingredients" },
                { label: "Good recipe" },
              ],
            },
            {
              label: "Good service (disabled node)",
              disabled: true,
              children: [
                { label: "Prompt attention" },
                { label: "Professional waiter" },
              ],
            },
            {
              label: "Pleasant surroundings",
              children: [
                { label: "Happy atmosphere" },
                { label: "Good table presentation" },
                { label: "Pleasing decor" },
              ],
            },
            {
              label: "Good food 2",
              children: [
                { label: "Quality ingredients" },
                { label: "Good recipe" },
              ],
            },
            {
              label: "Good service (disabled node) 2",
              disabled: true,
              children: [
                { label: "Prompt attention" },
                { label: "Professional waiter" },
              ],
            },
            {
              label: "Pleasant surroundings 2",
              children: [
                { label: "Happy atmosphere" },
                { label: "Good table presentation" },
                { label: "Pleasing decor" },
              ],
            },
          ],
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

    V3DApp.createApp("scene01.gltf", "scene01_logic.js");
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
    resetFilter() {
      this.filter = "";
      this.$refs.filter.focus();
    },
  },
};
</script>

<style>
@import "../v3dApp/app.css";
</style>
