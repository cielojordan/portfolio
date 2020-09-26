<template>
  <v-app>
    <Navbar />
    <div>
      <h2 class="text-capitalize">
        {{ photo.description ? photo.description : "Untitled" }}
      </h2>
    </div>
    <div>
      <v-img
        id="image"
        :src="photo.urls.regular"
        aspect-ratio="1"
        class="grey lighten-2"
      >
        <template v-slot:placeholder>
          <v-row class="fill-height ma-0" align="center" justify="center">
            <v-progress-circular indeterminate color="grey lighten-5" />
          </v-row>
        </template>
      </v-img>
    </div>
    <div class="d-flex justify-space-between">
      <v-responsive
        class="rounded-circle align-center justify-center ms-3"
      >
        <v-btn
          text
          color="grey"
          :to="
            `${
              prevPhotoIndex >= 0
                ? '/gallery/' + photos[prevPhotoIndex].id
                : '/gallery/' + photos[currPhotoIndex].id
            }`
          "
          nuxt
        >
          Previous
        </v-btn>
      </v-responsive>
      <div>
        <h4 class="text-capitalize">
          {{ photo.alt_description }}
        </h4>
      </div>
      <v-responsive
        class="text-xs-center rounded-circle align-center justify-center ms-3"
      >
        <v-btn
          text
          color="grey"
          :to="
            `${
              nextPhotoIndex <= 8
                ? '/gallery/' + photos[nextPhotoIndex].id
                : '/gallery/' + photos[currPhotoIndex].id
            }`
          "
          nuxt
        >
          Next
        </v-btn>
      </v-responsive>
    </div>
    <div>
      <h5 class="text-capitalize">
        Owned by:
      </h5>
      <span>{{ photo.user.name }}</span>
    </div>
    <div>
      <h5 class="text-capitalize">
        Portfolio:
      </h5>
      <span>
        <a :href="photo.user.portfolio_url">{{
          photo.user.portfolio_url
        }}</a></span>
    </div>
    <div>
      <h5 class="text-capitalize">
        Bio:
      </h5>
      <span>{{ photo.user.bio }}</span>
    </div>
    <div>
      <h5 class="text-capitalize">
        Socials:
      </h5>
      <span>
        <a
          :href="
            'https://www.instagram.com/' + photo.user.instagram_username
          "
        >Instagram</a>
        <a
          :href="'https://www.twitter.com/' + photo.user.twitter_username"
        >Twitter</a>
      </span>
    </div>
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
        return {
          photos: res.data.results,
          photo: res.data.results.find(el => el.id === params.id),
          currPhotoIndex: res.data.results.findIndex(el => el.id === params.id),
          prevPhotoIndex:
            res.data.results.findIndex(el => el.id === params.id) - 1,
          nextPhotoIndex:
            res.data.results.findIndex(el => el.id === params.id) + 1
        }
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

<style scoped>
div {
  text-decoration: none;
  text-align: center;
  margin-top: 1em;
}

#image {
  height: 20em;
  margin-left: "auto";
  margin-right: "auto";
}
</style>
