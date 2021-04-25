<template>
  <v-row>
    <v-col cols="12" sm="12" md="12">
      <v-parallax
        dark
        src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg"
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            class="text-center"
            cols="12"
          >
            <h1 class="display-1 font-weight-thin mb-4">
              {{ channel.name }}
            </h1>
            <h2>
              {{ channel.description }}
            </h2>
            <v-btn
              rounded
              color="primary"
              dark
              :href="channel.url"
              target="_blank"
            >
              {{ channel.url }}
            </v-btn>
          </v-col>
        </v-row>
      </v-parallax>
    </v-col>
    <v-col cols="12" sm="12" md="12">
      <v-container>
        <v-row>
          <v-col
            v-for="article in articles"
            :key="article.id"
            cols="12"
            md="4"
          >
            <v-card
              class="mx-auto"
              max-width="400"
            >
              <v-img
                class="white--text align-end"
                height="200px"
                :src="article.urlToImage"
              >
                <v-card-title>{{ article.title }}</v-card-title>
              </v-img>

              <v-card-subtitle class="pb-0">
                {{ article.author }}
              </v-card-subtitle>

              <v-card-text class="text--primary">
                <div>{{ article.publishedAt }}</div>

                <div>{{ article.description }}</div>
              </v-card-text>

              <v-card-actions>
                <v-list-item class="grow">
                  <v-row
                    align="center"
                    justify="start"
                  >
                    <v-icon class="mr-1">
                      mdi-heart
                    </v-icon>
                    <span class="subheading mr-2">0</span>
                    <span class="mr-1">·</span>
                    <v-icon class="mr-1">
                      mdi-share-variant
                    </v-icon>
                    <span class="subheading">0</span>
                  </v-row>
                  <v-btn
                    text
                    :href="article.url"
                    target="_blank"
                    justify="end"
                  >
                    Read More
                  </v-btn>
                </v-list-item>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-col>
  </v-row>
</template>

<script>
export default {
  inject: {
    theme: {
      default: { isDark: false }
    }
  },
  props: {
    channel: {
      type: [String, Array, Object],
      required: true,
      default: () => ({
        category: null,
        country: null,
        description: null,
        id: null,
        language: null,
        name: null,
        url: null
      })
    },
    articles: {
      type: [String, Array, Object],
      required: true
    }
  },
  data: () => ({
    dialog: false,
    notifications: false,
    sound: true,
    widgets: false,
    attrs: {
      class: 'mb-6',
      boilerplate: true,
      elevation: 2,
      type: 'table-heading, list-item-two-line, image, table-tfoot'
    }
  })
}
</script>
