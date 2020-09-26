<template>
  <nav>
    <v-toolbar class="grey lighten-2">
      <v-toolbar-title class="text-uppercase grey--text">
        <h2>
          <span
            class="pa-5"
          ><span class="font-weight-light">PORT</span><span class="font-weight-heavy">TKY</span></span>
        </h2>
      </v-toolbar-title>

      <template v-slot:extension>
        <v-tabs v-model="tab" grow active-class slider-color="red">
          <v-tab
            v-for="link in links"
            :key="link.text"
            class="red--text"
            :to="link.route"
            nuxt
          >
            {{ link.text }}
          </v-tab>
        </v-tabs>
      </template>
    </v-toolbar>
  </nav>
</template>

<script>
export default {
  data () {
    return {
      tab: null,
      links: [
        {
          text: 'Home',
          route: '/'
        },
        {
          text: 'Gallery',
          route: '/gallery'
        },
        {
          text: 'Contact',
          route: '/contact'
        }
      ]
    }
  }
}
</script>
