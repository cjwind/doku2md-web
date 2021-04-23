<template>
  <q-page>
    <q-input
      v-model="text"
      filled
      type="textarea"
      rows="30"
      @input="onInput"
    />
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      text: ''
    }
  },
  methods: {
    onInput () {
      console.log(this.text)
    }
  }
}
</script>
