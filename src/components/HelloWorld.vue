<template>
  <v-container>
    <v-container>
      <v-layout>
        <v-flex xs12 sm8 offset-sm2 lg6 offset-lg3>
          <h1 class="display-3 text-center">Hello</h1>
          <v-form v-model="valid" v-on:submit.prevent="fetchRepositories">
            <v-text-field v-model="query" label="Search GitHub" color="black" />
          </v-form>
          <v-btn block color="warning" rounded v-on:click="fetchRepositories"
            >Search</v-btn
          >
        </v-flex>
      </v-layout>
    </v-container>
    <v-container grid-list-md>
      <v-layout row wrap justify-center>
        <v-flex v-for="repo in repositories" :key="`${repo.id}`" xs12 sm8 lg6>
          <!-- Make card into own component -->
          <v-card>
            <v-container fluid>
              <v-layout row>
                <v-flex xs3>
                  <v-avatar tile size="100">
                    <img
                      :src="`${repo.owner.avatar_url}`"
                      :alt="`${repo.name}`"
                    />
                  </v-avatar>
                </v-flex>
                <v-flex xs9>
                  <v-card-text>
                    <a :href="`${repo.html_url}`" target="_blank">
                      <h3>{{ repo.name }}</h3>
                    </a>
                    <p>{{ repo.created_at }}</p>
                    <v-btn
                      small
                      color="primary"
                      v-on:click="fetchBranches(repo.name)"
                      >See branches</v-btn
                    >
                  </v-card-text>
                </v-flex>
              </v-layout>
              <template
                v-if="repo.name === repository.name && showBranches"
                transition="slide-y-transition"
              >
                <p
                  v-for="branch in repository.branches"
                  :key="`${branch.name}`"
                >
                  {{ branch.name }}
                </p>
              </template>
            </v-container>
          </v-card>
        </v-flex>
      </v-layout>
      <v-pagination v-model="page"></v-pagination>
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
    baseUrl: "https://api.github.com",
    query: "",
    page: 1,
    repositories: [],
    repository: {
      name: "",
      branches: []
    },
    showBranches: false
  }),
  methods: {
    async fetchRepositories() {
      try {
        if (!this.query) return;
        const response = await axios.get(
          `${this.baseUrl}/users/${this.query}/repos?type=owner&per_page=25&page=${this.page}`,
          { headers: { Accept: "application/vnd.github.v3+json" } }
        );
        this.repositories = response.data;
      } catch (err) {
        console.log("Error:", err);
      }
    },
    async fetchBranches(name: string) {
      this.showBranches = !this.showBranches;
      try {
        const response = await axios.get(
          `${this.baseUrl}/repos/${this.query}/${name}/branches`,
          { headers: { Accept: "application/vnd.github.v3+json" } }
        );
        this.repository.name = name;
        this.repository.branches = response.data;
      } catch (err) {
        console.log("Error:", err);
      }
    }
  }
});
</script>
