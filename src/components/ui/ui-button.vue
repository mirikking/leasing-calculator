<template>
    <button 
        :class="'ui-button'" 
        @click="proceed"
    >
        <h2 v-if="!this.isProceed" 
            class="ui-button-text"
        >
            <slot></slot>
        </h2>
        <svg v-else class="ui-button-preloading" width="21" height="21" viewBox="0 0 21 21" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M13.8691 19.3825C11.9455 20.1121 9.83718 20.1976 7.86093 19.6261C5.88468 19.0546 4.14734 17.857 2.91009 16.2134C1.67285 14.5698 1.00256 12.569 1.00001 10.5118C0.99745 8.45459 1.66276 6.45212 2.89591 4.80546C4.12906 3.15879 5.86342 1.95691 7.83825 1.38051C9.81307 0.804109 11.9216 0.88433 13.847 1.60911C15.7723 2.3339 17.4103 3.66408 18.5148 5.39968C19.6193 7.13528 20.1305 9.18251 19.9716 11.2336" stroke="white" stroke-width="2" stroke-linecap="round"/>
        </svg>
    </button>
</template>
<script>
import anime from 'animejs';
import axios from 'axios';

export default {
    name: "UiButton",
    data() {
        return {
            isProceed: false
        }
    },
    props: {
        carPrice: {
            type: Number,
            required: true
        },
        duration: {
            type: Number,
            required: true
        },
        percent: {
            type: Number,
            required: true
        },
        sumOfPayment: {
            required: true
        },
        mounthlyPayment: {
            required: true
        },
        initialPayment: {
            required: true
        }
    },
    methods: {
        proceed(event) {
            this.$emit('proceed', this.isProceed) // up info to the parent for interface lock
            this.animate().then(() => {
                let postData = {
                    car_coast: this.carPrice,
                    initail_payment: this.initialPayment,
                    initail_payment_percent: this.percent,
                    lease_term: this.duration,
                    total_sum: this.sumOfPayment,
                    monthly_payment_from: this.mounthlyPayment
                }
                axios.post('https://hookb.in/eK160jgYJ6UlaRPldJ1P', postData)
            })
        },
    animate(event) {
    return  anime({
            targets: ".ui-button-text",
            opacity: [
                {value: 0, duration: 200, easing: "linear"}
            ]
        }).finished.then(() => {
            this.isProceed = true;
        }).then(() => {
            anime({
                targets: ".ui-button-preloading",
                opacity: [
                    {value: 1, duration: 200, easing: "linear"}
                ]
            }).finished.then(() => {
                anime({
                    targets: ".ui-button-preloading",
                    rotate: 360,
                    loop: true,
                    duration: 800,
                    easing: "linear"
                    })
                }).then(() => {
                    anime({
                        targets: ".ui-button",
                        opacity: 0.4,
                        duration: 200,
                    })
                })
            })
        }
    }
}
</script>
<style scoped lang="scss">
@import '@/assets/styles/ui/button.scss';
    
</style>