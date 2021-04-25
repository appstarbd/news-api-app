<template>
  <v-app :dark="dark">
    <v-navigation-drawer
      v-model="drawer"
      :clipped="clipped"
      :fixed="fixed"
      app
    >
      <v-overlay
        absolute
        :value="loading"
      >
        <v-progress-circular
          indeterminate
        />
      </v-overlay>
      <v-list v-if="!loading">
        <v-list-item
          v-for="(source, index) in sources"
          :key="index"
          @click="readNews(source, index)"
        >
          <v-list-item-action>
            <v-avatar
              size="36px"
            >
              <img
                :alt="source.name"
                :src="`/images/${source.id}.png`"
              >
            </v-avatar>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="source.name" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title v-text="title" />
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container fluid>
        <channel v-if="articles !== null" :channel="channel" :articles="articles" />
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import Channel from '~/pages/channel.vue'
export default {
  components: {
    Channel
  },
  data () {
    return {
      dark: true,
      clipped: true,
      drawer: this.$vuetify.breakpoint.mdAndUp,
      fixed: false,
      loading: true,
      sources: {
        category: null,
        country: null,
        description: null,
        id: null,
        language: null,
        name: null,
        url: null
      },
      locale: 'en',
      languages: ['ar', 'de', 'en', 'es', 'fr', 'he', 'it', 'nl', 'no', 'pt', 'ru', 'se', 'ud', 'zh'],
      miniVariant: false,
      title: 'News App',
      channel: null,
      articles: null
    }
  },
  mounted () {
    this.newsSources()
  },
  methods: {
    newsSources (locale = 'en') {
      this.$axios.get('sources', {
        params: {
          language: locale
        }
      }).then((response) => {
        if (response.status === 200) {
          this.sources = response.data.sources
          const channel = this.sources[0]
          this.readNews(channel, 0)
          this.loading = false
        }
      })
    },
    readNews (channel, index) {
      this.channel = this.sources[index]
      const id = channel.id
      this.$axios.get('everything', {
        params: {
          sources: id
        }
      }).then((response) => {
        if (response.status === 200) {
          this.articles = response.data.articles
          this.loading = false
        }
      })
    }
  }
}
</script>
