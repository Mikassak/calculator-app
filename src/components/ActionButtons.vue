<template>
    <div>
        <div class="wrapper">
            <p 
                v-for="i in buttons" :key="i" 
                class="button" :class="{ 'equal' : i == '='}"
                v-on:click="actionPressed(i)"
            >
                {{ i }}
            </p>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                buttons: [ '+', '-', 'x', '/', 'C', '=' ]
            }
        },

        methods: {
            actionPressed(action) {
                this.$emit('handleAction', action)
            }
        },
    }
</script>

<style lang="scss" scoped>
.wrapper{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
}

.button{
    color: white;
    background-color: #515152;
    border-top: 1px solid #dfdfdf;
    border-left: 1px solid #dfdfdf;
    cursor: pointer;
    height: 100%;
    width: 100%;
    margin: auto;
}
.button:hover{
    background-color: #777778;
}
.equal{
    grid-column-start: 2;
    grid-column-end: 5;
}
</style>