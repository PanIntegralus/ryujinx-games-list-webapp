<script>
import SearchBar from './components/SearchBar.vue'
import ResultsContainer from './components/ResultsContainer.vue'
import axios from 'axios'

const owner = "Ryujinx"
const repo = "Ryujinx-Games-List"

export default {
  components: {
    SearchBar,
    ResultsContainer
  },
  data() {
    return {
      issues: []
    }
  },
  methods: {
    search(query) {
      const apiUrl = `https://api.github.com/search/issues?q=repo:${owner}/${repo}+${query}`
      
      let resultsFiltered = []
      axios.get(apiUrl)
      .then(response => {
        for (let i = 0; i < response.data.items.length; i++) {
            let regexResult = response.data.items[i].title.match("^(.*?) - ([A-Za-z0-9]+)$")
            if (regexResult) {
                console.log(response.data.items[i])
                let id = i
                let gameName = regexResult[1]
                let gameId = regexResult[2]
                let status = ""
                let htmlUrl = response.data.items[i].html_url
                let labels = []

                // FOR THE LOVE OF GOD CHANGE THIS ATROCITY OF CODE
                // SOME POINT IN THE FUTURE, PLEASE PLEASE PLEASE
                response.data.items[i].labels.forEach(element => {
                    labels.push(element.name)
                    if (element.url.endsWith("status-boots")) {
                        status = "boots"
                    } else if (element.url.endsWith("status-ingame")) {
                        status = "ingame"
                    } else if (element.url.endsWith("status-menus")) {
                        status = "menus"
                    } else if (element.url.endsWith("status-nothing")) {
                        status = "nothing"
                    } else if (element.url.endsWith("status-playable")) {
                        status = "playable"
                    }
                });

                resultsFiltered.push({
                    id,
                    gameName,
                    gameId,
                    status,
                    htmlUrl,
                    labels
                })
            }
        }
        // Asigna los resultados al arreglo 'issues'
        this.issues = resultsFiltered
      })
      .catch(err => {
        console.error(err)
      })
    }
  }
}
</script>

<template>
  <div>
    <SearchBar @search="search" />
    <ResultsContainer :issues="issues"/>
    <div id="version">development version 0.1</div>
  </div>
</template>
