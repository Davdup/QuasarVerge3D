<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="q-gutter-x-md">
        <q-btn
          id="myID"
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />
        <q-separator dark vertical inset/>
        <q-toolbar-title>
          Verge3D integration to Quasar App
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Main Menu
        </q-item-label>

<q-item
    clickable
    tag="a"
    to="/"
  >
    <q-item-section
      avatar
    >
      <q-icon name="home" />
    </q-item-section>

    <q-item-section>
      <q-item-label>Home</q-item-label>
    </q-item-section>
  </q-item>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'

const linksData = [
  {
    title: 'Scene 01',
    caption: 'A first 3D scene',
    icon: '3d_rotation',
    to: '/scene01'
  },
  {
    title: 'Scene 02',
    caption: 'A second 3D scene',
    icon: '3d_rotation',
    to: '/scene02'
  }
];

export default {
  name: 'MainLayout',
  components: { EssentialLink },
  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData
    }
  }
}
</script>
