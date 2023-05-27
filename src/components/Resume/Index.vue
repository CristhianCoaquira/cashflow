<template>
  <main>
    <p>{{ labelVisual }}</p>
    <h1>{{ amountCurrency }}</h1>
    <div class="graphic">
      <slot name="graphic"></slot>
    </div>
    <div class="action">
      <slot name="action"></slot>
    </div>
  </main>
</template>
<script>
import currencyFormatter from '../../js/currencyFormater'

export default {
  props: {
    label: String,
    totalAmount: Number,
    amount: Number
  },
  computed: {
    amountVisual() {
      return this.amount ?? this.totalAmount
    },
    labelVisual() {
      return this.label ?? 'Monto Total'
    },
    amountCurrency() {
      return currencyFormatter.format(this.amountVisual)
    }
  }
}
</script>
<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 14px;
  color: var(--brand-green);
}
.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>
