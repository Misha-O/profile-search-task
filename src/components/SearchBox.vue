<template>
    <div class="container">
        <form class="form">
            <div class="form__box">
                <i class="fas fa-search"></i>
                <input type="text" class="form__input" v-model.trim="searchInput" @input="filterInput" @keydown.enter="filterInput" />
            </div>
        </form>
        <div class="container__cards">
            <div v-if="selectedRecords.length" class="container__cards-selected" >
                <span v-for="(selected, index) in selectedRecords" :key="index">
                    <card :record="selected" :searchInput="searchInput" :cardSelected="cardSelected" @cardSelected="onCardSelected($event)" />
                </span>
            </div>
            <RecycleScroller class="scroller" :class="{'dynamic-height': selectedRecords.length}" :items="filteredRecords.length ? filteredRecords : records" key-field="email" :itemSize="170" v-slot="{ item }">
                <card :record="item" :searchInput="searchInput" @cardSelected="onCardSelected($event)" />
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
            selectedRecords: [],
            cardSelected: false
        }
    },
    methods: {
        filterInput() {
            this.filteredRecords = this.records.filter(record => {
                return record.name.toLowerCase().includes(this.searchInput.toLowerCase())
            })
        },
        onCardSelected(record) {
            if (this.selectedRecords.some(selected => selected.email === record.email)) {
                this.selectedRecords.splice(this.selectedRecords.indexOf(record), 1)
                this.cardSelected = false
            } else {
                this.selectedRecords.push(record)
                this.cardSelected = true
            }
        }
    },
    mounted() {
        this.filteredRecords = []
        this.selectedRecords = []
    }
}
</script>

<style lang="scss" scoped>
.container {
    width: 565px;
    height: 650px;
    max-height: 650px;
    background: #fff;
    padding: 13px;
}
.container__cards {
    height: 80%;

    &-selected {
        height: 35%;
        overflow-y: auto;
        margin-bottom: 20px;
    }
    .scroller {
        height: 100%;
        overflow-y: hidden;
        
        &.dynamic-height {
            height: 65%;
        }
    }
}
.form {
    & .form__box {
        display: flex;
        align-items: center;
        position: relative;

        & i {
            position: absolute;
            left: 8%;
            bottom: 40%;
            color: rgba(0, 0, 0, 0.5);
        }

        .form__input {
            width: 90%;
            height: 48px;
            padding-left: 50px;
            margin: 20px 30px;
            box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24);
            border-radius: 2px;
            border-color: transparent;
            outline: none;
            background: #fafafa;
            &:focus {
                border-color: transparent;
            }
        }
    }
}
</style>
