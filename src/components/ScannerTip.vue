<template>
  <el-dialog
    title="提示"
    :visible.sync="dialogVisible"
    width="30%"
    :before-close="handleClose"
  >
    <span>请扫描二维码...</span>
  </el-dialog>
</template>
<script>
export default {
  name: "ScannerTip",
  props: {
    showDialog: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      dialogVisible: false,
    };
  },
  methods: {
    handleClose(done) {
      this.dialogVisible = false;
      this.$emit("update:showDialog", false);
      done();
    },
  },
  watch: {
    showDialog(newVal, oldVal) {
      console.log("this.showDialog:", this.showDialog);
      this.dialogVisible = this.showDialog;
    },
  },
};
</script>