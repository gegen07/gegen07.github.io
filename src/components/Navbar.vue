<template>
  <v-toolbar color="primary" fixed app>
    <router-link to="/">
      <v-toolbar-title @click="$vuetify.goTo('#landing', { offset: -100 })" class="grey--text text--lighten-3">Germano Barcelos</v-toolbar-title>
    </router-link>
    <v-spacer></v-spacer>
    <v-toolbar-items class="fill-height">
      <!-- <v-btn class="grey--text text--lighten-3" to="/about" flat> Sobre </v-btn> -->
      <v-btn class="grey--text text--lighten-3" to="/blog" flat> Blog </v-btn>
      <v-btn class="grey--text text--lighten-3" flat target="_blank" v-bind:href="linkCV()">
        CV
      </v-btn>
      <v-menu bottom>
        <v-btn
          slot="activator"
          color="primary"
          dark
          flat
          class="grey--text text--lighten-3"
        >
          <v-toolbar-title class="body-1">{{ displayLocale }}</v-toolbar-title>
        </v-btn>

        <v-list>
          <v-list-tile
            v-for="(item, index) in langs"
            :key="index"
            @click="changeLocale(item)"
          >
            <v-list-tile-title>
              <v-layout row wrap>
                <v-flex xs1>
                  <p class="body-1">{{ item.name }}</p>
                </v-flex>
              </v-layout>
            </v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>
    </v-toolbar-items>
  </v-toolbar>
</template>

<script>
export default {
  name: 'navbar',
  data: () => ({
    langs: [
      { name: 'English', id: 'en' },
      { name: 'Português', id: 'pt_BR' }
    ]
  }),
  methods: {
    changeLocale (item) {
      this.$i18n.locale = item.id
    },
    linkCV: function () {
      const cvEN = 'https://drive.google.com/open?id=1rkQ3Hsk9yx1dcKVC1We9qaqDlUMSKbeQ'
      const cvPT = 'https://drive.google.com/open?id=1OQvs40bzM90_-gs3S0rtXJW3Lue9of8l'

      if (this.$i18n.locale === 'pt_BR') {
        return cvPT
      } else {
        return cvEN
      }
    }
  },
  computed: {
    displayLocale: function () {
      let other = 'en'
      if (this.$i18n.locale === 'pt_BR') {
        other = 'pt_BR'
      }
      return other
    }
  }
}
</script>

<style>

</style>
