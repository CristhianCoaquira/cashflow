<template>
  <div>
    <svg @touchstart="tap" @touchmove="tap" @touchend="untap" viewBox="0 0 300 200">
      <line stroke="#c4c4c4" stroke-width="2" x1="0" :y1="zero" x2="300" :y2="zero" />
      <polyline fill="none" stroke="#0689B0" stroke-width="2" :points="points" />
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="200"
      />
    </svg>
    <p>Ultimos 30 dias</p>
  </div>
</template>
<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  amounts: {
    type: Array,
    default: () => []
  }
})

const points = computed(() => {
  const total = props.amounts.length
  return props.amounts.reduce((previous, current, index) => {
    const x = (300 / total) * (index + 1)
    const y = amountToPixels(current)
    return `${previous} ${x},${y}`
  }, '0,100')
})

const zero = computed(() => {
  return amountToPixels(0)
})

const showPointer = ref(false)
const pointer = ref(0)

const emit = defineEmits(['select'])

const tap = ({ target, touches }) => {
  showPointer.value = true
  const elementWidth = target.getBoundingClientRect().width
  const elementX = target.getBoundingClientRect().x
  const touchX = touches[0].clientX
  pointer.value = ((touchX - elementX) * 300) / elementWidth
  emit('select', pointer.value)
}

const untap = () => {
  showPointer.value = false
}

const amountToPixels = (amount) => {
  const min = Math.min(...props.amounts)
  const max = Math.max(...props.amounts)
  const amountAbs = amount + Math.abs(min)
  const minmax = Math.abs(max) + Math.abs(min)
  return 200 - ((amountAbs * 100) / minmax) * 2
}
</script>
<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>
