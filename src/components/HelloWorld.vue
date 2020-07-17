<template>
  <v-container>
    <v-container>
      <v-layout>
        <v-flex xs12 sm8 offset-sm2 lg6 offset-lg3>
          <h1 class="display-3 text-center">Hello</h1>
          <v-form v-model="valid" v-on:submit.prevent="fetchResults">
            <v-text-field v-model="query" label="Search GitHub" color="black" />
          </v-form>
          <v-btn block color="warning" rounded v-on:click="fetchResults"
            >Search</v-btn
          >
        </v-flex>
      </v-layout>
    </v-container>
    <v-container grid-list-md>
      <v-layout row wrap justify-center>
        <v-flex
          v-for="repository in repositories"
          :key="`${repository.id}`"
          xs12
          sm8
          lg6
        >
          <v-card>
            <v-container fluid>
              <v-layout row>
                <v-flex xs 5>
                  <v-avatar tile>
                    <img
                      :src="`${repository.owner.avatar_url}`"
                      :alt="`${repository.name}`"
                    />
                  </v-avatar>
                </v-flex>
                <v-flex xs 7>
                  <v-card-text>
                    <a :href="`${repository.html_url}`" target="_blank">
                      <h3>{{ repository.name }}</h3>
                    </a>
                    <p>{{ repository.created_at }}</p>
                  </v-card-text>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card>
        </v-flex>
        <v-pagination v-model="page"></v-pagination>
      </v-layout>
    </v-container>
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
    page: 1,
    repositories: []
  }),
  methods: {
    async fetchResults() {
      try {
        if (!this.query) return;
        const response = await axios.get(
          `https://api.github.com/users/${this.query}/repos?type=owner&per_page=25&page=${this.page}`,
          { headers: { Accept: "application/vnd.github.v3+json" } }
        );
        this.repositories = response.data;
      } catch (err) {
        console.log("Error:", err);
      }
    }
  }
});
</script>
