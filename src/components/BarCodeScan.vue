<template>
  <div></div>
</template>
<script>
export default {
  name: "BarCodeScan",
  props: {},
  data() {
    return {
      realBarcode: "",
      keyupLastTime: undefined,
      regexRules: [],
    };
  },
  created() {
    this.initRegexRules();
    let that = this;
    // 监听页面的keyup事件
    document.onkeyup = function (e) {
      that.handleKeyUp(e);
    };
  },

  destroyed() {
    //取消键盘监听事件
    document.onkeyup = null;
  },

  methods: {
    // 初始化条码规则（自定义）
    initRegexRules() {
      this.regexRules = [
        {
          regex: "/^(10|11|12|13|14|15|16|17|18|19)[0-9]{16,17}/",
          value: "WX",
        },
        {
          regex: "/^(25|26|27|28|29|30)[0-9]{14,22}/",
          value: "ZFB",
        },
      ];
    },
    // 处理keyup事件
    handleKeyUp(e) {
      let gap = 0;
      if (this.keyupLastTime) {
        gap = new Date().getTime() - this.keyupLastTime;
        if (gap > 50) {
          gap = 0;
          this.realBarcode = "";
        }
      }
      this.keyupLastTime = new Date().getTime();
      // 输入法会触发keyup事件，key为Process，跳过即可
      if (e.key != "Process" && gap < 50) {
        if (e.key.trim().length == 1) {
          // 输入单个字母或者数字
          this.realBarcode += e.key;
        } else if (e.key.trim() == "Enter") {
          // 根据规则，判断barcode类型，返回数据（自定义规则）
          if (this.realBarcode) {
            let data = {
              type: this.barcodeRule(this.realBarcode),
              code: this.realBarcode,
            };
            this.$emit("handle", data);
            this.realBarcode = "";
          }
        }
      }
    },
    // 判断条码类型，如果没找到，则返回类型为barCode
    barcodeRule(barcode) {
      let value;
      this.regexRules.some((item, index) => {
        let regex = eval(item.regex);
        if (regex.test(barcode)) {
          value = item.value;
          return true;
        }
      });
      return value ? value : "UNKNOWN";
    },
  },
};
</script>
