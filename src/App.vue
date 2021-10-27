<template>
    <div id="app">
        <section>
            <div v-if="!showError">
                <div v-if="records.length" class="container">
                    <search-box :records="records" />
                </div>
                <template v-else>
                    <loading-spinner />
                </template>
            </div>
            <div v-else class="alert">
                <h3>Sorry, looks like the was a problem fetching it from the server. Please try again.</h3>
            </div>
        </section>
    </div>
</template>

<script>
import SearchBox from '@/components/SearchBox.vue'
import LoadingSpinner from '@/components/LoadingSpinner.vue'
import axios from 'axios'

export default {
    name: 'App',
    components: { SearchBox, LoadingSpinner },
    data() {
        return {
            records: [],
            uri: 'https://gist.githubusercontent.com/allaud/093aa499998b7843bb10b44ea6ea02dc/raw/c400744999bf4b308f67807729a6635ced0c8644/users.json',
            showError: false,
            perPage: 200,
            totalPages: 0,
            currentPage: 1,
        }
    },
    methods: {
        onPageChange(page) {
            console.log(page)
            this.currentPage = page
        }
    },
    async created() {
        try {
            const response = await axios.get(this.uri)
            this.totalPages = response.data.length / this.perPage
            console.log('respose: ', response)
            if (response.status === 200) {
                return (this.records = response.data.slice(0, 1000))
            } else {
                this.showError = true
            }
        } catch (error) {
            this.showError = true
        }
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,300;0,400;0,500;0,700;0,900;1,400;1,700&display=swap');
body {
    background: #eeeeee;
    margin: 0;
    padding: 0;
}
#app {
    font-family: 'Roboto', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
section {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 90px auto 120px;
}
.container {
    width: 565px;
    height: 650px;
    background: #fff;
    overflow-y: scroll;
}

.alert {
    width: 40%;
    margin: 100px auto;
    padding: 30px;
    position: relative;
    border-radius: 5px;
    background-color: #f7a7a3;
    box-shadow: 0 0 15px 5px #ccc;
}
</style>
