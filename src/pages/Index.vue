<template>
  <q-page>
    <div class="q-pa-md" style="width: 80%">
      Dokuwiki
      <q-input
        v-model="text"
        filled
        type="textarea"
        rows="20"
        @input="onInput"
      />

      Markdown
      <q-input
        v-model="markdown"
        filled
        type="textarea"
        rows="20"
      />
    </div>
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
        line = this.convertHeader(line)
        line = this.convertMonospaced(line)
        line = this.convertCodeTag(line)
        line = this.convertLink(line)
        line = this.convertUnorderedListItem(line)
        line = this.convertOrderedListItem(line)
        line = this.convertItalic(line)
        markdown += line + '\n'
      })
      return markdown
    },
    convertHeader (line) {
      return line.replace(/====== (.*) ======/, '# $1')
        .replace(/===== (.*) =====/, '## $1')
        .replace(/==== (.*) ====/, '### $1')
        .replace(/=== (.*) ===/, '#### $1')
        .replace(/== (.*) ==/, '##### $1')
        .replace(/= (.*) =/, '###### $1')
    },
    convertMonospaced (line) {
      return line.replace(/''%%(.*)%%''/, '`$1`')
    },
    convertCodeTag (line) {
      return line.replace(/<code( *)(.*)>/, '```$2')
        .replace(/<\/code>/, '```')
        .replace(/<sxh( *)(.*)>/, '```$2')
        .replace(/<\/sxh>/, '```')
    },
    convertLink (line) {
      return line.replace(/\[\[(.*)\|(.*)]]/, '[$2]($1)')
    },
    convertUnorderedListItem (line) {
      return line.replace(/^  ( *)\* (.*)/, '$1* $2')
    },
    convertOrderedListItem (line) {
      return line.replace(/^  ( *)- (.*)/, '$11. $2')
    },
    convertItalic (line) {
      return line.replace(/(http(s*):)\/\//, '__$1_placeholder__')
        .replace(/\/\/(.*)\/\//, '*$1*')
        .replace(/__(http(s*):)_placeholder__/, '$1//')
    },
    onInput () {
      this.markdown = this.convert(this.text)
    }
  }
}
</script>
