<template>
    <span>
        <div>
            <form name="search" class="form">
                <input type="text" class="form__input" label="Search..." v-model.trim="searchInput" @input="filterInput" @keydown.enter="filterInput" />
            </form>
        </div>
        <template v-if="searchInput === '' && !hasResult">
            <div v-for="(record, index) in records" :key="index">
                <card :record="record"/>
            </div>
        </template>

        <template v-else>
            <div v-for="(record, index) in filteredRecords" :key="index">
                <card :record="record" :searchInput="searchInput"/>
            </div>
        </template>
    </span>
</template>

<script>
import Card from '@/components/Card.vue'

export default {
    name: 'search-box',
    components: { Card },
    props: {
        records: {
            type: [Array],
            required: false
        }
    },
    data() {
        return {
            searchInput: '',
            filteredRecords: [],
            hasResult: false
        }
    },
    methods: {
        filterInput() {
            this.filteredRecords = this.records.filter(record => {
                return record.name.toLowerCase().includes(this.searchInput.toLowerCase())
            })
            this.hasResult = true
        },
        highlightSearch(search) {
            if (!this.searchInput && !this.searchInput.includes(search)) {
                return false
            } else {
                const regex = new RegExp(this.searchInput, 'gi')
                let result = search.replace(regex, str => {
                    return '<span style="background-color:yellow;">' + str + '</span>'
                })
                return result
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.form {
    display: flex;
    align-items: center;
}
.form__input {
    width: 90%;
    height: 48px;
    padding-left: 50px;
    margin: 20px 30px;
    box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24);
    border-radius: 2px;
    background: url('/icon-search.png') no-repeat scroll 7px 7px;
}
</style>
