<template>
  <div class="page-content">
    <div class="row">
      <div>À chaque fois que</div>
      <div><input type="text" v-model="trigger"></div>
      <div>je dépose</div>
      <div><input type="number" v-model="amount"> $</div>
      <div>dans la tirelire.</div>
    </div>
    <div><button @click="increaseCounter" v-text="selectedCounterButtonLabel">+1</button></div>
    <div>Tu dois déposer <span class="total">{{ totalDue }}$</span> dans ta tirelire (ou sur ton compte) la prochaine fois</div>
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
      counter: 0
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
  },
  watch: {
    trigger (val) {
      localStorage.trigger = val
    },
    amount (val) {
      localStorage.amount = val
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

      @media only screen and (min-width: 768px) {
        flex-direction: row;
      }
      > div {
        padding: 20px;

        @media only screen and (min-width: 768px) {
          padding: 0;
        }
        &:not(:last-child) {
          @media only screen and (min-width: 768px) {
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
  }
</style>
