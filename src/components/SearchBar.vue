<script>
    export default {
        expose: ['results'],
        props: {
            results: { type: Array }
        },
        data() {
            return {
                query: ''
            }
        },
        methods: {
            search() {
                this.$emit('search', this.query)
            }
        }
    }

</script>

<template>
    <div id="searchBar">
        <input v-model="query" @keyup.enter="search" type="text" placeholder="Search for game name or ID..." maxlength="256"/>
        <button @click="search">🔎</button>
    </div>
</template>

<style scoped>
#searchBar {
  display: flex;
  align-content: center;
  justify-content: center;
  text-align: center;
}

#searchBar input {
  width: 50vw;
  margin: 5px;
  padding: 0;
}

#searchBar input:focus {
  outline: none;
}
</style>