<template>
  <v-app class="grey darken-4">
    <Navbar />
    <v-card>
      <v-container fluid>
        <v-row>
          <v-col
            v-for="photo in photos.results"
            :key="photo.id"
            class="d-flex child-flex justify-center"
            cols="4"
          >
            <v-card
              shaped
              class="d-flex"
              outlined
              :to="'/gallery/' + photo.id"
              nuxt
            >
              <Photo
                :id="photo.id"
                :thumbnail="photo.urls.regular"
                :title="photo.description"
                :caption="photo.alt_description"
              />
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-card>
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
      photos: [],
      numPics: 9
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
