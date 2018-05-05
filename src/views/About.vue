<template>
    <my-page title="关于" backable>
        <div>印章制作 v1.0.1</div>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
            }
        },
        mounted() {
            // this.$http.get('/version').then(
            //     response => {
            //         let data = response.data
            //         if (data.code === 200) {
            //             console.log(data)
            //             this.version = data.data
            //         }
            //     },
            //     response => {
            //         console.log(response)
            //     })
        }
    }
</script>

<style scoped>
</style>
