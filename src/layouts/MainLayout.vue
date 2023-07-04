<template>
  <q-layout dark view="hHh Lpr lff">
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          GloboList
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
        <q-btn
          @click="showMenu = !showMenu"
          class="q-ma-sm"
          round
          flat
        >
          <q-menu>
            <div class="row no-wrap q-pa-md">
              <div class="column">
                <div class="text-h6 q-mb-md">Preferences</div>
                  <q-toggle
                    :model-value="darkMode"
                    @update:model-value="toggleDarkMode"
                    label="Dark mode"
                    left-label
                  />
                </div>
              <q-separator vertical inset class="q-mx-lg" />
              <div class="column items-center">
                <q-avatar size="72px">
                  <img src="https://cdn.quasar.dev/img/avatar4.jpg">
                </q-avatar>

                <div class="text-subtitle1 q-mt-md q-mb-xs">John Doe</div>

                <q-btn
                  color="primary"
                  label="Logout"
                  push
                  size="sm"
                  v-close-popup
                />
              </div>
            </div>
          </q-menu>
          <q-icon name="account_circle" />
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawer"
      overlay
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Essential Links
        </q-item-label>

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
import { defineComponent, ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import { useQuasar } from 'quasar'
const linksList = [
  {
    title: 'Docs',
    caption: 'quasar.dev',
    icon: 'school',
    link: 'https://quasar.dev'
  },
  {
    title: 'Github',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework'
  },
  {
    title: 'Discord Chat Channel',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'https://chat.quasar.dev'
  },
  {
    title: 'Forum',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev'
  },
  {
    title: 'Twitter',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'https://twitter.quasar.dev'
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev'
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev'
  }
]

export default defineComponent({
  name: 'MainLayout',
  components: {
    EssentialLink
  },
  setup () {
    const $q = useQuasar();
    const leftDrawer = ref(true);
    const darkMode = ref(true);
    const showMenu = ref(false);
    try{
      if($q.localStorage.has("leftDrawer")){
        leftDrawer.value = $q.localStorage.getItem("leftDrawer")
      }
      else{
          $q.localStorage.set("leftDrawer", leftDrawer.value)
      }
      if($q.localStorage.has("darkMode")) {
          darkMode.value = $q.localStorage.getItem("darkMode")
      }else{
          $q.localStorage.set("darkMode", darkMode.value)
      }
    }
    catch (e) {
      console.log("Error getting prefs locally: " + e.toString())
    }
    $q.dark.set(darkMode.value)
    return {
      leftDrawer,
      darkMode,
      showMenu,
      essentialLinks: linksList,
      toggleDarkMode() {
        darkMode.value = !darkMode.value;
        $q.localStorage.set("darkMode", darkMode.value)
        $q.dark.toggle();
      },
      toggleLeftDrawer () {
        leftDrawer.value = !leftDrawer.value
        $q.localStorage.set("leftDrawer", leftDrawer.value)
      }
    }
  }
})
</script>
