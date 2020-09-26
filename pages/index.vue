<template>
  <v-app class="grey darken-4">
    <Navbar />
    <v-main class="mt-4 mb-4 pa-10">
      <v-container>
        <v-layout row wrap>
          <v-flex xs12 md6>
            <v-carousel hide-delimiters>
              <v-carousel-item
                v-for="photo in photos.results"
                :key="photo.id"
                :src="photo.urls.regular"
                :to="'/gallery/' + photo.id"
                reverse-transition="fade-transition"
                transition="fade-transition"
                nuxt
              />
            </v-carousel>
          </v-flex>
          <v-flex xs12 md6>
            <h2 class="pa-5">
              ABOUT
            </h2>
            <h1 class="pl-5">
              TITLE
            </h1>
            <p class="pl-5">
              Lorem ipsum dolor, sit amet consectetur adipisicing elit. Hic,
              sapiente porro rerum quasi maiores molestiae voluptatibus, nulla
              quos nemo atque quisquam, provident pariatur amet! Explicabo vero
              neque qui consectetur autem!
            </p>
            <div class="pa-5">
              <v-btn to="/contact/">
                CONTACT US
              </v-btn>
            </div>
          </v-flex>
        </v-layout>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { toJson } from 'unsplash-js'
import axios from 'axios'

export default {
  asyncData ({ params, error }) {
    return axios
      .get(
        'https://api.unsplash.com/search/photos/?client_id=wfuovsAGbP_7PkT2TTv12eVNgFcjry-Xn2zl9E1-wXU&query=tokyo&per_page=9'
      )
      .then((res) => {
        return { photos: res.data }
      })
      .catch((e) => {
        // eslint-disable-next-line no-console
        console.log(e)
      })
  },
  data () {
    return {
      photos: []
    }
  },
  methods: {
    async getPhotos () {
      return await this.unsplash.search
        .photos('tokyo')
        .then(toJson)
        .then((json) => {
          this.photos = json
        })
    }
  }
}
</script>
