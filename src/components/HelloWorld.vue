<template>
  <v-container>
    <v-alert v-model="error" type="warning" dismissible>
      No results found. Please try again
    </v-alert>
    <v-container mt-5>
      <v-layout>
        <v-flex xs12 sm8 offset-sm2 lg6 offset-lg3>
          <h1 class="display-2 mb-5 mt-5 text-center">Search GitHub</h1>
          <v-spacer></v-spacer>
          <v-form v-model="valid" v-on:submit.prevent="fetchRepositories" mt-5>
            <v-text-field v-model="query" label="Search GitHub" color="black" />
            <v-btn block color="warning" rounded type="submit">Search</v-btn>
          </v-form>
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
                <v-flex xs4 sm3>
                  <v-avatar tile size="100">
                    <img
                      :src="`${repo.owner.avatar_url}`"
                      :alt="`${repo.name}`"
                    />
                  </v-avatar>
                </v-flex>
                <v-flex xs8 sm9>
                  <v-card-text>
                    <h3>
                      <a :href="`${repo.html_url}`" target="_blank" mb-3>
                        {{ repo.name }}
                      </a>
                    </h3>
                    <p>Created: {{ displayDate(repo.created_at) }}</p>
                    <v-btn
                      small
                      outlined
                      color="primary"
                      v-on:click="fetchBranches(repo.name)"
                      >Show branches</v-btn
                    >
                  </v-card-text>
                </v-flex>
                <v-flex
                  v-if="repo.name === repository.name && showBranches"
                  transition="slide-y-transition"
                  xs8
                  offset-xs4
                  sm9
                  offset-sm3
                >
                  <v-card-text>
                    <p
                      v-for="branch in repository.branches"
                      :key="`${branch.name}`"
                    >
                      {{ branch.name }}
                    </p>
                  </v-card-text>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card>
        </v-flex>
      </v-layout>
      <v-container>
        <v-layout>
          <v-flex row justify-space-around xs12 sm8 offset-sm2 align-center>
            <v-btn
              v-if="page >= 2"
              v-on:click="handlePageDecrease"
              color="primary"
              >Previous</v-btn
            >
            <v-btn
              v-if="checkRepositories()"
              v-on:click="handlePageIncrease"
              color="primary"
              >Next</v-btn
            >
          </v-flex>
        </v-layout>
      </v-container>
    </v-container>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
import axios from 'axios'
export default Vue.extend({
  name: 'HelloWorld',
  data: () => ({
    valid: false,
    baseUrl: 'https://api.github.com',
    query: '',
    page: 1,
    repositories: [],
    repository: {
      name: '',
      branches: []
    },
    showBranches: false,
    error: false
  }),
  methods: {
    async fetchRepositories() {
      try {
        if (!this.query) {
          this.page = 1
          return
        }
        const response = await axios.get(
          `${this.baseUrl}/users/${this.query}/repos?type=owner&per_page=25&page=${this.page}`,
          { headers: { Accept: 'application/vnd.github.v3+json' } }
        )
        this.repositories = response.data
      } catch (err) {
        console.log(err)
        this.error = true
      }
    },
    async fetchBranches(name: string) {
      this.showBranches = !this.showBranches
      try {
        const response = await axios.get(
          `${this.baseUrl}/repos/${this.query}/${name}/branches`,
          { headers: { Accept: 'application/vnd.github.v3+json' } }
        )
        this.repository.name = name
        this.repository.branches = response.data
      } catch (err) {
        console.log(err)
      }
    },
    handlePageIncrease() {
      if (this.repositories.length === 0) return
      this.page++
      this.fetchRepositories()
    },
    handlePageDecrease() {
      if (this.page > 1) {
        this.page--
        this.fetchRepositories()
      }
    },
    displayDate(date: string) {
      return new Date(date).toLocaleDateString()
    },
    checkRepositories() {
      return this.repositories.length > 24
    }
  }
})
</script>
