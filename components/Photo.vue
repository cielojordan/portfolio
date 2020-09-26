<template>
  <v-img
    :src="thumbnail"
    aspect-ratio="2"
    class="grey lighten-2"
    max-width="400px"
  >
    <template v-slot:placeholder>
      <v-row class="fill-height ma-0" align="center" justify="center">
        <v-progress-circular indeterminate color="grey lighten-5" />
      </v-row>
    </template>
  </v-img>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['thumbnail', 'title', 'caption', 'id', 'owner']
}
</script>

<style scoped></style>
