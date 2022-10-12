<template>
    <div :class="'ui-inputs'">
        <div class="ui-input-content">
            <input v-if="!this.isComputing" 
                class="ui-input-number" 
                type="number" 
                v-model="this.currentSum" 
                :min="this.minSum" 
                :max="this.maxSum" 
                @input="limit(); fillBg(); changedPrice()" 
                @click="changeBg" 
                @focusout="changeBg"
            >
            <input v-else 
                class="ui-input-number" 
                type="number" 
                disabled 
                v-model="this.computedFirstPaySum" 
                @input="limit(); fillBg(); changedPrice()"
            >
            <h2 
                v-if="!this.isComputing" 
                class="ui-input-additional"
            >
                <slot></slot>
            </h2>
            <div v-else 
                class="ui-input-additional-computing"
            >
                <input 
                    class="ui-input-additional-input" 
                    type="number" 
                    v-model="this.currentSum" 
                    :step="this.step" 
                    :min="this.minSum" 
                    :max="this.maxSum" 
                    @input="fillBg(); changedPrice(); limit()" 
                    @click="changeBg" 
                    @focusout="changeBg">
                <h3>%</h3>
            </div>
        </div>
        <input 
            class="ui-input-range" 
            type="range" 
            v-model="this.currentSum" 
            :step="this.step" 
            :min="this.minSum" 
            :max="this.maxSum" 
            @input="fillBg(); changedPrice()"
        >
    </div>
</template>
<script>
export default {
    name: "UiInput",
    data() {
        return {
            currentSum: this.minSum,
            computedFirstPaySum: 100000 //
        }
    },
    props: {
        minSum: {
            type: Number,
            required: true,
        },
        maxSum: {
            type: Number,
            required: true,
        },
        step: {
            type: Number,
            required: false,
        },
        isComputing: {
            type: Boolean,
            required: false,
            default: false
        },
        carPrice: {
            type: Number,
            required: false,
        }

    }, 
    watch: {
        carPrice() {
            this.changedPrice()
        }
    },
    methods: {
        fillBg(event) {
            let percentage = (this.currentSum - this.minSum) / (this.maxSum - this.minSum) * 100;
            this.$el.querySelector('.ui-input-range').style = 'background: linear-gradient(to right, #FF9514, #FF9514 ' + percentage + '%, #E1E1E1 ' + percentage + '%, #E1E1E1 100%)';
        },
        limit(event) {
            if (this.currentSum > this.maxSum) {
                this.currentSum = this.maxSum
            }
            if (this.currentSum < this.minSum) {
                this.currentSum = this.minSum
            }
        },
        changedPrice(event) {
            this.$emit("getPrice", this.currentSum)
            this.$emit("getPercent", this.currentSum)
            this.$emit("getDuration", this.currentSum)
            this.computedFirstPaySum = Math.round(this.currentSum * (1/100) * this.carPrice)
        },
        changeBg(event) {
            if (!this.$el.classList.contains("active")) {
                this.$el.classList.add("active")
            } else {
                this.$el.classList.remove("active")
            }
        }
    }
}
</script>
<style scoped lang="scss">
@import '@/assets/styles/ui/input.scss';
@import "@/assets/styles/@core/screens/desktop/1440.scss";
@import "@/assets/styles/@core/screens/desktop/1120.scss";
@import "@/assets/styles/@core/screens/tablet/768.scss";
@import "@/assets/styles/@core/screens/mobile/320.scss";
</style>