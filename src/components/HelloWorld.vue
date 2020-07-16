<template>
  <v-container>
    <v-layout>
      <v-flex xs12 sm8 offset-sm2 lg6 offset-lg3>
        <v-card color="green">
          <h3 class="text-center">Hello</h3>
          <v-card-text>
            <v-form v-model="valid" v-on:submit.prevent="fetchResults">
              <v-text-field
                v-model="query"
                label="Search GitHub"
                color="black"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn right color="warning" rounded v-on:click="fetchResults"
              >Search</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  name: "HelloWorld",

  data: () => ({
    valid: false,
    query: "",
    repositories: []
  }),
  methods: {
    async fetchResults() {
      try {
        const response = await axios.get(
          "https://api.github.com/users/airbnb/repos?type=owner&per_page=25&page=1",
          { headers: { Accept: "application/vnd.github.v3+json" } }
        );
        console.log(response.data);
      } catch (err) {
        console.log("error", err);
      }
    }
  }
});
</script>
