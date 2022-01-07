<template>
    <div>
    
        <div class="container">
            <div class="displey">
                <p>{{ displeyValue }}</p>
            </div>

            <div>
                <action-buttons @handleAction="makeAction"></action-buttons>
            </div>

            <div>
                <number-buttons @handleNumber="addNumber"></number-buttons>
            </div>
            
        </div>

        <transition name="fade">
            <div v-if="history.length">
                <h3>history: </h3>

                <transition-group name="slide-fade">
                    <div v-for="(item, index) in history" :key="index + ' history'" class="history">
                            
                        {{ item.n1 }} {{ item.operator}} {{ item.n2 }} = {{ item.result }}
                        
                    </div>
                </transition-group>
            </div>
        </transition>
    </div>
</template>

<script>
import ActionButtons from './ActionButtons.vue';
import NumberButtons from './NumberButtons.vue'
export default {
  components: { NumberButtons, ActionButtons },
    data() {
        return {
            displeyValue: 0,
            value: null,
            prevValue: null,
            operator: '',
            result: null,
            history: []
        }
    },

    methods: {
        addNumber(number) {

            if (this.displeyValue == 0){
                this.displeyValue = number;
            }
            else {
                // make it string
                this.displeyValue = "" + this.displeyValue + number;
            }
              
        },

        makeAction(action) {

            switch(action){
                case "+": 
                case "-": 
                case "x": 
                case "/": this.handleOperator(action);
                    break;
                case "C": this.clearDisplay();
                    break;
                case "=": this.equal();
                    break;
            }
        },


        clearDisplay(){
            this.displeyValue = 0;
            this.prevValue = null;
            this.result = null;
        },

        handleOperator(z){
            this.operator = z;
            this.prevValue = this.displeyValue;
            this.displeyValue = 0;
        },

        equal() {
            // make it number
            let num2 = + this.displeyValue;
            let num1 = + this.prevValue;

            // save new vlue 
            this.value = num1;

            if (this.operator === "+"){
                this.result = num1 + num2;
            }
            else if (this.operator === "-"){
                this.result = num1 - num2;
            }
            else if (this.operator === "x"){
                this.result = num1 * num2;
            }
            else if (this.operator === "/"){
                if (num2 == 0){
                    throw "MATH ERROR: Cannot divide by 0";
                }
                this.result = num1 / num2;
            }

            this.displeyValue = this.result;
            
            let obj= {};

            obj.n1 = num1;
            obj.n2 = num2;
            obj.operator = this.operator;
            obj.result = this.result;

            this.history.push(obj);
        }
            
    },

}
</script>

<style lang="scss" scoped>
.container{
    width: 220px;
    margin: auto;
    box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px, rgba(0, 0, 0, 0.3) 0px 8px 16px -8px;
}

.displey p{
    color: white;
    background-color: #303042;
    border-top-left-radius: 9px;
    border-top-right-radius: 9px;
    height: 100%;
    margin: 0;
    padding: 15px 0;
}


/* Enter and leave Transitions */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>