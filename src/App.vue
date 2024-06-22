<template>
  <div id="app" style="margin-top: 12px">
    <el-button @click="showInputDialog = true" type="danger" style="margin-bottom: 12px">输入JSON字符串</el-button>
    <JsonEditor :value="jsonData" @node-clicked="handleNodeClicked"/>
    <el-input
        style="margin-top: 12px"
        type="textarea"
        :autosize="{ minRows: 5, maxRows: 7 }"
        placeholder="选取的属性"
        v-model="selectedFieldNames">
    </el-input>
    <el-dialog
        title="JSON输入"
        :visible.sync="showInputDialog"
        width="80%"
        @close="inputJsonData = null"
        center
        :close-on-click-modal="false"
    >
      <el-input
          type="textarea"
          :autosize="{ minRows: 7, maxRows: 12 }"
          placeholder="请输入内容"
          v-model="inputJsonData">
      </el-input>
      <span slot="footer" class="dialog-footer">
        <el-button @click="showInputDialog = false">取 消</el-button>
        <el-button type="primary" @click="dialogConfirm">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import JsonEditor from './components/JSONEditor.vue'

export default {
  name: 'App',
  components: {
    JsonEditor
  },
  data() {
    return {
      jsonData: {},
      inputJsonData: null,
      showInputDialog: false,
      selectedFieldNames: ''
    }
  },
  methods: {
    handleNodeClicked(path) {
      this.selectedFieldNames += path + '\n'
    },
    dialogConfirm () {
      this.jsonData = JSON.parse(this.inputJsonData)
      this.showInputDialog = false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
