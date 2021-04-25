<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12" md="12">
      <v-overlay :value="loading">
        <v-progress-circular
          indeterminate
          size="64"
        />
      </v-overlay>

      <v-container v-show="headlines.status === 'ok'">
        <v-carousel v-model="model" :hide-delimiters="$vuetify.breakpoint.smAndDown">
          <v-carousel-item
            v-for="(headline, index) in headlines.articles"
            :key="index"
            :src="headline.urlToImage"
            reverse-transition="fade-transition"
            transition="fade-transition"
            eager
          >
            <v-container
              fill-height
              fluid
              pa-0
              ma-0
              pb-10
            >
              <v-layout fill-height align-end pb-4 mb-4>
                <v-flex xs12>
                  <v-card color="red" class="pa-2">
                    <span class="headline white--text" v-text="headline.title" />
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>
          </v-carousel-item>
        </v-carousel>
      </v-container>

      <v-container>
        <v-row>
          <v-col
            cols="12"
            md="4"
          >
            <v-skeleton-loader
              v-bind="attrs"
              type="card-avatar, article, actions"
            />

            <v-skeleton-loader
              v-bind="attrs"
              type="date-picker"
            />
          </v-col>

          <v-col
            cols="12"
            md="4"
          >
            <v-skeleton-loader
              v-bind="attrs"
              type="article, actions"
            />

            <v-skeleton-loader
              v-bind="attrs"
              type="table-heading, list-item-two-line, image, table-tfoot"
            />
          </v-col>

          <v-col
            cols="12"
            md="4"
          >
            <v-skeleton-loader
              v-bind="attrs"
              type="list-item-avatar, divider, list-item-three-line, card-heading, image, actions"
            />

            <v-skeleton-loader
              v-bind="attrs"
              type="list-item-avatar-three-line, image, article"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-col>
  </v-row>
</template>

<script>
// import Logo from '~/components/Logo.vue'
// import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    // Logo,
    // VuetifyLogo
  },
  data: () => ({
    news: [],
    articles: [],
    status: null,
    totalResults: 0,
    headlines: {
      articles: [],
      status: null,
      totalResults: 0
    },
    loading: true,
    attrs: {
      class: 'mb-6',
      boilerplate: true,
      elevation: 2
    },
    model: 0,
    colors: [
      'primary',
      'secondary',
      'yellow darken-2',
      'red',
      'orange'
    ]
  }),
  watch: {
    loading (val) {
      val && setTimeout(() => {
        this.loading = false
      }, 3000)
    }
  },
  mounted () {
    this.topHeadlines()
  },
  methods: {
    topHeadlines (source = 'bbc-news') {
      this.$axios.get('top-headlines', {
        params: {
          sources: source
        }
      }).then((response) => {
        // console.dir(response)
        if (response.status === 200) {
          // this.news = response.data
          // this.articles = response.data.articles
          // this.status = response.data.status
          // this.totalResults = response.data.totalResults
          this.headlines = response.data
          this.loading = false
        }
      }).catch((errors) => {
        console.error(errors)
      })
    },
    showChannel () {
      console.dir('Hello world')
    }
  }
}
</script>
