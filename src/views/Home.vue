<template>
  <div class="home">
    <AutoButton @click="handleClick" type="error">Yes!!!!!</AutoButton>
  </div>
</template>

<script>
import AutoButton from '../components/auto-button'

export default {
  name: 'home',
  components: {
    AutoButton
  },
  methods: {
    handleClick () {
      console.log('button is clicked.')
    }
  }
}
</script>
