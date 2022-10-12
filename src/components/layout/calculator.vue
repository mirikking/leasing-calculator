<template>
    <main class="calculator-main">
        <div class="offer">
            <h1 class="offer-msg">Рассчитайте стоимость автомобиля в лизинг</h1>
        </div>
        <div class="config-offer">
            <div class="car-price">
                <p>Стоимость автомобиля</p>
                <UiInput
                :class="'ui-inputs'"
                @getPrice="getPrice" 
                :carPrice="this.carPrice" 
                :minSum="1000000" 
                :maxSum="6000000" 
                :step="50000" 
                >₽</UiInput>
            </div>
            <div class="first-pay">
                <p>Первоначальный взнос</p>
                <UiInput 
                :class="'ui-inputs'"
                @getPercent="getPercent" 
                :carPrice="this.carPrice" 
                :minSum="10" 
                :maxSum="60" 
                :step="1" 
                :isComputing="true"
                ></UiInput>
            </div>
            <div class="leasing-time">
                <p>Срок лизинга</p>
                <UiInput 
                :class="'ui-inputs'"
                @getDuration="getDuration" 
                :carPrice="this.carPrice" 
                :minSum="1" 
                :maxSum="60" 
                :step="1"
                >мес.</UiInput>
            </div>
        </div>
        <div class="price-offer">
            <div class="sum-offer">
                <p>Сумма договора лизинга</p>
                <h1> {{ this.sumOfPayment }} ₽</h1>
            </div>
            <div class="monthly-pay">
                <p>Ежемесячный платеж от</p>
                <h1> {{this.mounthlyPayment}} ₽</h1>
            </div>
            <div class="request-offer">
                <UiButton 
                :class="'ui-button'"
                :carPrice="this.carPrice" 
                :duration="this.duration" 
                :percent="this.percent" 
                :sumOfPayment="this.sumOfPayment"
                :mounthlyPayment="this.mounthlyPayment"
                :initialPayment="this.initialPayment"
                @proceed="lockInterface"
                >Оставить заявку
                </UiButton>
            </div>
        </div>
    </main>
</template>
<script>
export default {
    name: "Calculator",
    data() {
        return {
            carPrice: 1000000, // default value
            duration: 1, // default value
            percent: 10, // default value
            sumOfPayment: Number,
            mounthlyPayment: Number,
            initialPayment: Number,
        }
    },
    mounted() {
        this.showPaymentSum()
    },
    methods: {
        getPrice(sum) {
            this.carPrice = Number(sum); 
            this.showPaymentSum()
        },
        getDuration(duration) {
            this.duration = Number(duration);
            this.showPaymentSum()
        },
        getPercent(percent) {
            this.percent = Number(percent);
            this.showPaymentSum()
        },
        showPaymentSum() {
            this.initialPayment = Math.round(this.percent * (1/100) * this.carPrice)
            this.mounthlyPayment = Math.round((this.carPrice - Math.round(this.percent * (1/100) * this.carPrice)) * (((this.percent * 0.01) * Math.pow((1 + (this.percent * 0.01)), this.duration)) / (Math.pow((1 + (this.percent * 0.01)), this.duration) - 1)));
            this.sumOfPayment = Math.round(this.percent * (1/100) * this.carPrice) + (this.duration * this.mounthlyPayment)
        },
        lockInterface() {
            this.$el.querySelector('.ui-button').disabled = true;
            this.$el.querySelectorAll('.ui-inputs').forEach(el => {
                el.classList.add('disabled')
                el.querySelector('.ui-input-number').disabled = true;
                el.querySelector('.ui-input-range').disabled = true;
            });

        }
    }

}
</script>
<style scoped lang="scss">
@import "@/assets/styles/@core/layout/calculator.scss";
@import "@/assets/styles/@core/screens/desktop/1440.scss";
@import "@/assets/styles/@core/screens/desktop/1120.scss";
@import "@/assets/styles/@core/screens/tablet/768.scss";
@import "@/assets/styles/@core/screens/mobile/320.scss";
@import "@/assets/styles/@core/screens/mobile/319.scss";
</style>