<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume :label="label" :total-amount="10000" :amount="amount">
        <template #graphic>
          <Graphic :amounts="amounts"></Graphic>
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
      amount: null,
      movements: [
        {
          id: 1,
          title: 'Movimiento',
          description: 'Deposito de salario',
          amount: 1000,
          time: new Date('2023-06-04')
        },
        {
          id: 2,
          title: 'Movimiento 1',
          description: 'Deposito de honorarios',
          amount: 500,
          time: new Date('2023-06-04')
        },
        {
          id: 3,
          title: 'Movimiento 3',
          description: 'Comida',
          amount: -100,
          time: new Date('2023-06-05')
        },
        {
          id: 4,
          title: 'Movimiento 4',
          description: 'Colegiatura',
          amount: -500,
          time: new Date('2023-06-07')
        },
        {
          id: 5,
          title: 'Movimiento 5',
          description: 'Reparación equipo',
          amount: 500,
          time: new Date('2023-06-07')
        },
        {
          id: 6,
          title: 'Movimiento 6',
          description: 'Reparación equipo',
          amount: -1500,
          time: new Date('2023-05-06')
        }
      ]
    }
  },
  computed: {
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date()
          const oldDate = today.setDate(today.getDate() - 30)
          return m.time > oldDate
        })
        .map((m) => m.amount)
      return lastDays.map((_, i, lastDays) => {
        const lastMovements = lastDays.slice(0, i + 1)
        return lastMovements.reduce((accAmount, amount) => accAmount + amount, 0)
      })
    }
  },
  methods: {
    create(movement) {
      this.movements.push(movement)
    },
    remove(id) {
      const index = this.movements.findIndex((m) => m.id === id)
      this.movements.splice(index, 1)
    }
  }
}
</script>
