<template>
    <div class="container">
        <div>
            <form name="search" class="form">
                <input type="text" class="form__input" label="Search..." v-model.trim="searchInput" @input="filterInput" @keydown.enter="filterInput" />
            </form>
        </div>
        <div class="container__cards">
            <RecycleScroller class="scroller" :items="filteredRecords.length ? filteredRecords: records" :item-size="32" key-field="email" itemSize="160"  v-slot="{ item }">
                <div class="user">
                    <card :record="item" :searchInput="searchInput" />
                </div>
            </RecycleScroller>
        </div>
    </div>
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
        }
    },
    methods: {
        filterInput() {
            this.filteredRecords = this.records.filter(record => {
                return record.name.toLowerCase().includes(this.searchInput.toLowerCase())
            })
        },
    }
}
</script>

<style lang="scss" scoped>
.container {
    width: 565px;
    height: 650px;
    background: #fff;
    padding: 13px;
}
.container__cards {
    height: 100%;
}
.scroller {
    height: 100%;
    overflow-y: hidden;
}
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
