<template>
    <div class="spinner__container">
        <div class="spinner__loader"></div>
    </div>
</template>

<script>
export default {
    name: 'ui-loading-spinner'
}
</script>

<style lang="scss">
.spinner__container {
    display: flex;
    justify-content: center;
    margin: 200px auto;
}
.spinner__loader {
    border: 8px solid #f3f3f3;
    border-radius: 50%;
    border-top: 8px solid #47cf75;
    width: 40px;
    height: 40px;
    -webkit-animation: spin 1s linear infinite; /* Safari */
    animation: spin 1s linear infinite;
}

/* Safari */
@-webkit-keyframes spin {
    0% {
        -webkit-transform: rotate(0deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
    }
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}
</style>
