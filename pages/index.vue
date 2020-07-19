<template>
  <section>

    <v-row>
      <v-col cols="10">
      <v-text-field v-model="searchWord" label="検索しよーぜ🔎"></v-text-field>
      </v-col>
      <v-col cols="2">
      <v-btn color="primary" depressed @click="searchQiita">検索</v-btn>
      </v-col>
    </v-row>

    <div>
      <p v-if="initialDisplay">検索KWDをいれてね</p>
      <div v-else-if="loadingStatus">
        <v-progress-circular indeterminate color="primary"></v-progress-circular>
      </div>
      <div v-else>
      <v-list>
        <v-subheader>検索結果</v-subheader>
        <v-list-item-group color="primary">
          <v-list-item
            v-for="(article, i) in data"
            :key="i"
          >
            <v-list-item-content>
                <v-list-item-title>
                  {{ i + 1 }}. <a :href="article.url">{{ article.title }}</a>
                  👍: {{ article.likes_count }}
                  💬: {{ article.comments_count }}
                </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data: function() {
    return {
      searchWord: null,
      initialDisplay: true,
      data: null,
      loadingStatus: false
    }
  },
  methods: {
    searchQiita: function() {
      this.initialDisplay = false;
      this.loadingStatus = true;
      this.$axios
        .get(`https://qiita.com/api/v2/items?page=1&per_page=30&query=${this.searchWord}`)
        .then(function(responce) {
          this.data = responce.data;
        }.bind(this))
        .catch(function(error) {
          console.log(error);
        })
        .finally(function() {
          this.loadingStatus = false;
        }.bind(this))
    }
  }
}
</script>

<style>
</style>
