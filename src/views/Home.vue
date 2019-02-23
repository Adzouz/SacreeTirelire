<template>
  <div class="page-content">
    <div class="row">
      <div>À chaque fois que</div>
      <div><input type="text" v-model="trigger"></div>
      <div>je dépose</div>
      <div><input type="number" v-model="amount"> $</div>
      <div>dans la tirelire.</div>
    </div>
    <div>
      <button :class="['increase-counter', animated ? 'animated' : '']" @click="increaseCounter">
        <span class="button-content">{{ selectedCounterButtonLabel }} <span class="plusone">+1</span></span>
      </button>
    </div>
    <div>Tu dois déposer <span class="total">{{ totalDue }}$</span> dans ta tirelire<br>(ou sur ton compte)</div>
    <div><button @click="resetCounter">J'ai déposé</button></div>
  </div>
</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      trigger: '',
      amount: 0,
      listCounterButtonLabels: [
        'Coupable votre honneur',
        'Oops j\'ai recommencé',
        'Promis celle là c\'était la dernière',
        'Oui mais là quand même j\'avais une bonne raison',
        'À ce train là, je vais pouvoir me payer un an de voyage',
        'Ok, celle là je l\'accorde',
        'J\'utilise ce bouton bien trop souvent'
      ],
      selectedCounterButtonLabel: null,
      counter: 0,
      animated: false
    }
  },
  computed: {
    totalDue () {
      return this.amount * this.counter
    }
  },
  methods: {
    increaseCounter () {
      this.counter++
      this.animated = true
      setTimeout(() => {
        this.animated = false
      }, 200)
      this.selectedCounterButtonLabel = this.listCounterButtonLabels[Math.floor(Math.random() * this.listCounterButtonLabels.length)]
    },
    resetCounter () {
      this.counter = 0
    }
  },
  mounted () {
    this.selectedCounterButtonLabel = this.listCounterButtonLabels[Math.floor(Math.random() * this.listCounterButtonLabels.length)]
    if (localStorage.trigger) {
      this.trigger = localStorage.trigger
    }
    if (localStorage.amount) {
      this.amount = localStorage.amount
    }
    if (localStorage.counter) {
      this.counter = localStorage.counter
    }
  },
  watch: {
    trigger (val) {
      localStorage.trigger = val
    },
    amount (val) {
      localStorage.amount = val
    },
    counter (val) {
      localStorage.counter = val
    }
  }
}
</script>

<style lang="scss">
  @import '../assets/stylesheets/variables';

  .page-content {
    font-size: 2rem;
    font-weight: $font-weight-light;

    .row {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;

      @media only screen and (min-width: 1024px) {
        flex-direction: row;
      }
      > div {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: row;
        padding: 20px;

        @media only screen and (min-width: 1024px) {
          width: auto;
          padding: 0;
        }
        &:not(:last-child) {
          @media only screen and (min-width: 1024px) {
            margin-right: 20px;
          }
        }
      }
    }
    > div {
      padding: 20px;
    }
    .total {
      font-size: 2.5rem;
      color: $illusion;
    }
    .increase-counter {
      position: relative;
      width: 350px;
      max-width: 100%;
      border-bottom: 0;
      background-color: transparent;
      background: url(../assets/img/piggy-bank.svg) no-repeat 50% 0%;
      background-size: 100% auto;

      &.animated {
        background-position: 50% 100%;

        &::after {
          transform: translateY(100px);
        }
      }
      &:focus,
      &:active {
        margin-top: 0;
        box-shadow: none;
      }
      &::before {
        content: '';
        display: block;
        width: 100%;
        padding-bottom: 73%;
      }
      &::after {
        content: '';
        position: absolute;
        top: -30px;
        left: 50%;
        transform: translateX(-50%);
        display: block;
        width: 40px;
        height: 40px;
        border-radius: 20px;
        background-color: $marzipan;
        border-bottom: 3px solid $goldenSand;
        z-index: -1;
        transition: .2s all ease-out;
      }
      .button-content {
        position: absolute;
        top: 50%;
        left: 50px;
        right: 70px;
        transform: translateY(-50%);
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
      }
      .plusone {
        display: block;
        font-weight: $font-weight-bold;
        font-size: 3rem;
        line-height: 1;
        position: absolute;
        top: 100%;
        left: 50%;
        transform: translateX(-50%);

        @media only screen and (max-width: 767px) {
          display: none;
        }
      }
    }
  }
</style>
