<template>
    <div class="card">
        <div class="card__avatar">
            <template  v-if="record.avatar">
                <img class="card__avatar-img" src="/card-img-bg.png" alt="Avatar placeholder" />
                <img class="card__avatar-img" :src="record.avatar" alt="Avatar" />
            </template>
            <template  v-else>
                <img class="card__avatar-img" src="/card-img-placeholder.png" alt="Avatar placeholder" />
            </template>
        </div>
        <div class="card__info">
            <div class="card__info-header">
                <h3 class="card__info-header--name" v-html="searchInput === '' ? record.name : highlightSearch(record.name)"/>
                <p class="card__info-header--email" v-html="searchInput === '' ? record.email : highlightSearch(record.email)"/>
            </div>
            <div class="card__info-body">
                <h6 class="card__info-body--title" v-html="searchInput === '' ? record.title : highlightSearch(record.title)"/>
                <p class="card__info-body--address" v-html="searchInput === '' ? record.address : highlightSearch(record.address)"/>
                <hr class="card__info--divider">
            </div>
            <div class="card__actions">
                <button class="card__actions-btn--select" @click="$emit('cardSelected', record)">
                    <template v-if="!cardSelected">Mark as Suitable</template>
                    <template v-else>Skip Selection</template>
                </button>
            </div>
        </div>
    </div>
</template>

<script>


export default {
    name: 'card',
    props: {
        record: {
            type: [Object],
            required: true
        },
        searchInput: {
            type: [String],
            required: false,
            default: ""
        },
        cardSelected: {
            type: [Boolean],
            required: false,
            default: false
        },
    },
    methods: {
        highlightSearch(search) {
            if (!this.searchInput && !this.searchInput.includes(search)) {
                return false
            } else {
                const regex = new RegExp(this.searchInput, 'gi')
                return search.replace(regex, str => {
                    return '<span style="background-color:yellow;">' + str + '</span>'
                })
            }
        },
    }
}
</script>

<style lang="scss" scoped>
.card {
    display: flex;
    margin-right: 15px;
    background: #fafafa;
    border: 1px solid #4765ff;
    box-sizing: border-box;
    box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24);
    border-radius: 3px;
}

.card__avatar {
    position: relative;
    height: 140px;
    width: 140px;

    & .card__avatar-img {
        position: absolute;
        bottom: 0;
        left: 0;
        height: 100%;
        width: 100%;
    }
}
.card__info {
    flex: 1;
    padding: 0 10px 10px 30px;

    .card__info-header {
        display: flex;
        justify-content: space-between;
        margin-top: 10px;

        &--name {
            word-wrap: break-word;
            padding: 0 1px;
            margin: 0;
            text-align: left;
            font-size: 24px;
            font-weight: 400;
            color: rgba(0, 0, 0, 0.87);
        }
        &--email {
            margin: 0;
            font-size: 14px;
            color: rgba(0, 0, 0, 0.54);
        }
    }

    .card__info-body {
        text-align: left;
        line-height: 20px;
        
        &--title {
            margin: 0;
            font-size: 14px;
            font-weight: 700;
            color: rgba(0, 0, 0, 0.55);
        }
        &--address {
            margin: 0;
            font-size: 14px;
            font-weight: 400;
            color: rgba(0, 0, 0, 0.54);
        }

        &--divider {
            border-top: 1px solid rgba(0, 0, 0, 0.12);
        }
    }

    .card__actions {

        &-btn--select {
            display: inline-block;
            border: none;
            background-color: inherit;
            padding: 5px;
            cursor: pointer;
            text-transform: uppercase;
            color: #009688;
            font-weight: 500;
            line-height: 16px;
            font-size: 14px;
            &:hover {
                opacity: 0.8;
            }
        }
    }
}
</style>
