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
      text: '',
      markdown: ''
    }
  },
  methods: {
    convert (doku) {
      let lines = doku.split('\n')
      let markdown = ''
      lines.forEach(line => {
        markdown += this.convertHeader(line) + '\n'
      })
      return markdown
    },
    convertHeader (line) {
      return line.replace('====== ', '# ')
        .replace('===== ', '## ')
        .replace('==== ', '### ')
        .replace('=== ', '#### ')
        .replace('== ', '##### ')
        .replace('= ', '###### ')
    },
    onInput () {
      this.markdown = this.convert(this.text)
      console.log(this.markdown)
    }
  }
}
</script>
