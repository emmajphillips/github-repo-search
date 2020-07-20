# github-repo-search

## Overview

This application was created using Vue.js, Vuetify, TypeScript and Axios to access the GitHub API. It was my first time working with Vue and the Vuetify UI Library.

The purpose of the app was to be able to search for all GitHub repositories associated with a username; display the repositories' name, the user's avatar and the date the repo was created; and provide the option to see each repository's branches.

Overall, I really enjoyed building this and felt it was a solid introduction to the Vue framework.

## Table of contents

* [Brief](#Brief)
* [Build](#Build)
* [Wins and challenges](#Wins-and-challenges)
* [Bugs and improvements](#Bugs-and-improvements)
* [Conclusion](#In-summary)

## Brief

* Create a Vue.js, Vuetify and TypeScript class component application using Axios to display a list of GitHub repositories.
* Use the Vue CLI
* Use the GitHub API
* Application can be built solely within the HelloWorld component
* Display results paginated (max 25 per page) showing the repository name, avatar and creation date
* Allow the user to choose a repository and view a list of its branches

## Build

I started by determining the required components for the application to work:
* Form with search input which captured query and passed it along to the GitHub user repositories endpoint
* Display containing search results (max 25 per page)
* Option on each repository to see respective repo's branches (i.e. a separate show component). This would use the branches endpoint of the GitHub API.

Next, I tested the two endpoints that I would need. I decided for the repositories endpoint to add two queries: the first that only returned repositories created by that particular user, and the second that automatically limited the number of results that were returned to 25. My reason for this is that the maximum amount of results the API would return without the query was 100, but this did not account for users with more than 100 repositories. By limiting the results, I had better control over displaying results for users with a large amount of repositories associated with their account. This is shown in the `fethRepositories` method:

```
const response = await axios.get(
          `${this.baseUrl}/users/${this.query}/repos?type=owner&per_page=25&page=${this.page}`,
          { headers: { Accept: 'application/vnd.github.v3+json' } }
        )
        this.repositories = response.data
```

With both endpoints working properly, I then set about writing the various components, capturing the search query through two-way binding, displaying results and allowing users to see all branches associated with a given repo.

## Wins and challenges

### Win

The biggest win was using Vue and Vuetify for the first time. I was thoroughly impressed with the two way binding of Vue, which, coming from working with React, was a borderline revelation.

### Challenge

I got myself into quite a bit of trouble with displaying the branches of the repositories. This was because I was trying to render them using a `v-for` while already in another `v-for`. It resulted in every result displaying the branches of whichever repository had been selected. I was able to address this issue by restructuring the data and the process for how it was stored in order to produce a more robust `v-if` statement that controlled which branches were displayed based on user interaction.

## Bugs and improvements

### Bugs

As this was my first time working with two of the technologies involved, there were bound to be some bugs. While I was able to resolve most (e.g. error handling for when a user doesn't exist, logic for page next/previous button, etc.), there is still one with the 'See branches' button:

```
<v-btn
  small
  outlined
  color="primary"
  v-on:click="fetchBranches(repo.name)"
  >Show branches</v-btn
>
```

Currently, it toggles between showing and hiding the branches of a given repository. The bug arises when a button is clicked on one repository and then another is clicked in a second repo. Instead of showing the branches of the second one, it only closes the first. From a user experience, this could certainly be improved by creating method to better control these events.

### Future improvements

One big improvement would be to break the current component into smaller ones. The most obvious being the results display. It is currently beign rendered by a `v-card` component, which could easily be moved into its own separate .vue file and imported into `HelloWorld`.

## In summary

I felt this was a great first introduction to Vue.js and Vuetify, and I hope to continue to learn more about developing in this particular JS framework.

## Setup

### Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```
