<template>
  <v-app class="grey darken-4">
    <Navbar />
    <v-main class="mx-4 mb-4 pa-10">
      <v-container class="my-5">
        <v-layout row wrap justify-center>
          <v-flex xs12 md6>
            <v-form ref="form" class="px-3">
              <v-text-field
                :v-model="firstName"
                label="First Name"
                required
                :rules="inputRules[0]"
              />
              <v-text-field
                v-model="lastName"
                label="Last Name"
                required
                :rules="inputRules[0]"
              />
              <v-text-field
                v-model="email"
                label="E-mail"
                required
                :rules="inputRules[1]"
              />
              <v-textarea
                v-model="inquiry"
                :counter="150"
                label="Inquiry"
                required
                :rules="inputRules[2]"
              />

              <div class="text-center">
                <v-dialog v-model="dialog" width="500">
                  <template v-slot:activator="{ on }">
                    <v-btn color="grey darken-2" dark v-on="on">
                      SUBMIT
                    </v-btn>
                  </template>

                  <v-card>
                    <v-card-title class="headline grey">
                      Inquiry Sent
                    </v-card-title>

                    <v-card-text class="pa-5 font-weight-medium">
                      {{ message }}
                    </v-card-text>

                    <v-divider />

                    <v-card-actions>
                      <v-spacer />
                      <v-btn color="primary" text @click="dialog = false">
                        OKAY
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </div>
            </v-form>
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
      photos: [],
      firstName: '',
      lastName: '',
      email: '',
      inquiry: '',
      dialog: false,
      inputValid: false,
      message:
        'Thank you for contacting us. Our customer service team will send a response via email as soon as possible. Have a great day!',
      // emailChecker: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
      inputRules: [
        [v => (v && v.length >= 1) || 'Please enter a valid name.'],
        [
          v =>
            (v && this.validateEmail(v)) ||
            'Please enter a valid email address.'
        ],
        [v => (v && v.length >= 10) || 'Please enter a valid inquiry.']
      ]
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
    },
    validateEmail (email) {
      // eslint-disable-next-line no-useless-escape
      const re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(email)
    }
  }
}
</script>

<style scoped>
form {
  background-color: grey;
  color: white;
  padding: 2em;
}
</style>
