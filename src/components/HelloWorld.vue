<template>
  <div class="hello">
    <el-row>
      <el-button type="primary" @click="onScanner()">扫描二维码</el-button>
      <div>
        <el-input
          ref="barCodeInput"
          v-model="output"
          @keyup.enter.native="completed"
          placeholder="请输入内容"
          type="textarea"
          rows="6"
          style="width: 460px"
        ></el-input>
      </div>
    </el-row>
    <bar-code-scan ref="barCodeScan" @handle="handleBarcode"></bar-code-scan>
    <scanner-tip :showDialog.sync="dialogVisible"></scanner-tip>
  </div>
</template>

<script>
import BarCodeScan from "./BarCodeScan";
import ScannerTip from "./ScannerTip";

export default {
  name: "HelloWorld",
  components: {
    BarCodeScan,
    ScannerTip,
  },
  props: {
    msg: String,
  },
  data() {
    return {
      output: "",
      count: 0,
      dialogVisible: false,
    };
  },

  created() {},

  mounted() {},

  methods: {
    completed() {
      //这里进行扫码枪扫码后的操作，调后台接口
      console.log("completed:", 12345);
    },
    onScanner() {
      this.dialogVisible = true;
      console.log("dialogVisible:", this.dialogVisible);
    },
    handleBarcode(barcodeMap) {
      console.log(barcodeMap);
      this.output = ++this.count + ":" + JSON.stringify(barcodeMap);
      this.dialogVisible = false;
      this.$nextTick(() => {
        this.$refs.barCodeInput.focus();
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
