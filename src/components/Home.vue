<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume :label="label" :total-amount="totalAmount" :amount="amount">
        <template #graphic>
          <Graphic :amounts="amounts" :label-date="labelDate" @select="select"></Graphic>
        </template>
        <template #action>
          <Action @create="create"></Action>
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movements="movements" @remove="remove"></Movements>
    </template>
  </Layout>
</template>
<script>
import Layout from './Layout.vue'
import Header from './Header.vue'
import Resume from './Resume/Index.vue'
import Movements from './Movements/Index.vue'
import Action from './Action.vue'
import Graphic from './Resume/Graphic.vue'

export default {
  components: {
    Layout,
    Resume,
    Movements,
    Header,
    Action,
    Graphic
  },
  data() {
    return {
      label: null,
      labelDate: null,
      amount: null,
      movements: []
    }
  },
  computed: {
    lastMovements() {
      return this.movements.filter((m) => {
        const today = new Date()
        const oldDate = today.setDate(today.getDate() - 30)
        return m.time > oldDate
      })
    },
    amounts() {
      const lastDays = this.lastMovements.map((m) => m.amount)
      return lastDays.map((_, i, lastDays) => {
        const lastMovements = lastDays.slice(0, i + 1)
        return lastMovements.reduce((accAmount, amount) => accAmount + amount, 0)
      })
    },
    totalAmount() {
      return this.movements.reduce((previous, current) => {
        return previous + current.amount
      }, 0)
    }
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem('movements'))
    if (Array.isArray(movements)) {
      this.movements = movements.map((m) => {
        return { ...m, time: new Date(m.time) }
      })
    }
  },
  methods: {
    create(movement) {
      this.movements.push(movement)
      this.save()
    },
    remove(id) {
      const index = this.movements.findIndex((m) => m.id === id)
      this.movements.splice(index, 1)
      this.save()
    },
    save() {
      localStorage.setItem('movements', JSON.stringify(this.movements))
    },
    select(amount, index = null) {
      this.amount = amount
      if (amount !== null && amount !== undefined) {
        this.label = this.lastMovements[index - 1].title
        const options = {
          year: 'numeric',
          month: 'long',
          day: 'numeric',
          hour: 'numeric',
          minute: 'numeric'
        }
        this.labelDate = this.lastMovements[index - 1].time.toLocaleString(undefined, options)
      } else {
        this.label = null
        this.labelDate = null
      }
    }
  }
}
</script>
