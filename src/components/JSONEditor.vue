<template>
  <div>
    <div ref="jsonEditor" class="json-editor"></div>
  </div>
</template>

<script>
import JSONEditor from 'jsoneditor'
import 'jsoneditor/dist/jsoneditor.css'

export default {
  name: "JSONEditor",
  props: {
    value: {
      type: Object,
      required: true
    }
  },
  mounted() {
    const options = {
      mode: 'tree',
      onEvent: (node, event) => {
        if (event.type === 'click') {
          let path = []
          for (let i = 0; i < node.path.length; i++) {
            if (typeof node.path[i] !== 'number') {
              path.push(node.path[i])
            }
          }
          this.$emit('node-clicked', path.join('.'))
        }
      }
    }
    this.editor = new JSONEditor(this.$refs.jsonEditor, options)
    this.editor.set(this.value)
  },
  methods: {

  },
  watch: {
    value(newValue) {
      this.editor.update(newValue)
    }
  },
  beforeDestroy() {
    if (this.editor) {
      this.editor.destroy()
    }
  }
}
</script>

<style scoped>
/deep/ .json-editor > .jsoneditor > .jsoneditor-outer > .jsoneditor-tree > .jsoneditor-tree-inner {
  max-height: 300px;
}
</style>
