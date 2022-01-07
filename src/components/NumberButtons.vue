<template>
    <div>
        <div class="wrapper">
            <p 
                v-for="i in buttons" :key="i" 
                class="button" :class="{ 'zero' : i == 0}"
                v-on:click="numberPressed(i)"
            >
                <span>
                    {{ i }}
                </span>
            </p>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                buttons: [ 7, 8, 9, 4, 5, 6, 1, 2, 3, 0 ]
            }
        },

        methods: {
            numberPressed(number) {
                this.$emit('handleNumber', number)
            }
        },
    }
</script>

<style lang="scss" scoped>
.wrapper{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
}

.button{
    background-color: #767682;
    color: white;
    border-top: 1px solid #dfdfdf;
    border-left: 1px solid #dfdfdf;
    cursor: pointer;
    width: 100%;
    margin: auto;
}
.button:hover{
    background-color: #95959c;
}
.zero{
    grid-column-start: 1;
    grid-column-end: 4;
}
</style>