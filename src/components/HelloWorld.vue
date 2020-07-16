<template>
  <v-container>
    <v-layout>
      <v-flex xs12 sm8 offset-sm2 lg6 offset-lg3>
        <v-card color="green">
          <h3 class="text-center">Hello</h3>
          <v-card-text>
            <v-form v-model="valid">
              <v-text-field
                v-model="query"
                label="Search GitHub"
                color="black"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn right color="warning" round>Search</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "HelloWorld",

  data: () => ({
    valid: false,
    query: ""
  })
});
</script>
