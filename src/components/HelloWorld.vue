<template>
  <div>
    <div class="head">
      <div class="eve">
        <div class="exc" v-on:click="exe">执行</div>
        <div class="fom" v-on:click="formatter">格式化</div>
      </div>
    </div>
    <div class="in-coder-panel">
      <div class="tables">
        <div v-for="(table, index) in tables" :key="index">
          <div class="table">
            <span>{{ index }}:</span>
            <div v-for="(col, V) in table" :key="V" class="table_item">{{
              col
            }}</div>
          </div>
        </div>
      </div>
      <div class="right">
        <div style="padding: 1px;border-top:1px solid cadetblue;transform: translate3d(0px, 0px, 0px);"></div>
        <textarea ref="textarea"></textarea>
        <div class="data">
          <!--表头-->
          <div class="data-head">
            <div
              class="data-head-column"
              v-for="(column, index) in dataHead"
              :key="index"
            >
              {{ column }}
            </div>
          </div>
          <!--表头-->
          <div
            class="data-item"
            v-for="(data_item, index) in dataItems"
            :key="index"
          >
            <div
              class="data-item-column"
              v-for="(data_item_column, cIndex) in data_item"
              :key="cIndex"
            >
              {{ data_item_column }}
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="footer">
      <div class="exc" v-on:click="exportCsv">下载CSV</div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
//发送请求相关
import axios from "axios";
// axios 配置
axios.defaults.timeout = 5000;
axios.defaults.baseURL = "http://localhost:3000";

import _CodeMirror from "codemirror";
// 核心样式
import "codemirror/lib/codemirror.css";
// 引入主题后还需要在 options 中指定主题才会生效
import "codemirror/mode/sql/sql.js";
// import the styles
import "@riophae/vue-treeselect/dist/vue-treeselect.css";
import "codemirror/addon/hint/show-hint.css";
import "codemirror/addon/hint/show-hint.js";
import "codemirror/addon/hint/sql-hint.js";
import { format } from "sql-formatter";

// 尝试获取全局实例
const CodeMirror = window.CodeMirror || _CodeMirror;

@Component({
  components: {},
})
export default class HelloWorld extends Vue {
  @Prop() private value: string = "";
  //数据列表 start
  private dataItems: any = []; //[[],[]]
  private dataHead: any = [];
  //数据列表 end

  //表集合
  private tables: any = {};

  //编辑器 配置属性 start
  // 编辑器绑定的值
  private code: any = "";
  // 默认的语法类型
  private mode: any = "sql";
  // 编辑器实例
  private coder: any = null;

  // 默认配置
  private options: any = {
    indentWithTabs: true,
    smartIndent: true,
    lineNumbers: true,
    matchBrackets: true,
    autofocus: false,
    styleSelectedText: true,
    tabSize: 2, // 缩进格式
    // theme: 'rubyblue', // 主题，对应主题库 JS 需要提前引入
    line: true,
    styleActiveLine: true, // 高亮选中行
    hintOptions: {
      completeSingle: true, // 当匹配只有一项的时候是否自动补全
    },
    mode: { name: "text/x-mysql" },
    extraKeys: {
      "'a'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'b'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'c'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'d'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'e'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'f'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'g'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'h'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'i'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'j'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'k'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'l'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'m'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'n'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'o'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'p'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'q'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'r'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'s'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'t'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'u'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'v'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'w'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'x'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'y'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'z'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'.'": function completeAfter(cm, pred) {
        if (!pred || pred())
          setTimeout(function() {
            if (!cm.state.completionActive)
              cm.showHint({
                completeSingle: false,
              });
          }, 100);
        return CodeMirror.Pass;
      },
      "'='": function completeIfInTag(cm) {
        return this.completeAfter(cm, function() {
          let tok = cm.getTokenAt(cm.getCursor());
          if (
            tok.type == "string" &&
            (!/['"]/.test(tok.string.charAt(tok.string.length - 1)) ||
              tok.string.length == 1)
          )
            return false;
          let inner = CodeMirror.innerMode(cm.getMode(), tok.state).state;
          return inner.tagName;
        });
      },
      "Ctrl-Enter": "autocomplete",
      Tab: function(cm) {
        let spaces = Array(cm.getOption("indentUnit") + 1).join(" ");
        cm.replaceSelection(spaces);
      },
    },
  };

  //编辑器 配置属性 end

  created() {}

  mounted() {
    this._initialize();
  }

  destroyed() {}

  //格式化代码
  formatter() {
    this.coder.setValue(format(this.code));
  }

  //执行sql
  exe() {
    let that = this;
    axios
      .get("/sql")
      .then((res) => {
        that.dataItems = res.data.data.dataItems;
        that.dataHead = res.data.data.dataHead;
        return res.data.data;
      })
      .catch((err) => {
        alert(err.message);
      });
    // axios.post('/sql', {"sql": this.code}).then(res => {
    //     that.dataItems = res.data.data.dataItems;
    //     that.dataHead = res.data.data.dataHead;
    //     return res.data.data
    // }).catch(err => {
    //     alert(err.message)
    // })
  }

  exportCsv() {
    if (this.dataHead.length == 0) {
      return;
    }

    let bomHead = "\uFEFF";
    let report_csv: string = bomHead;
    let head_csv = "";
    this.dataHead.forEach(function(item) {
      head_csv += item + ",";
    });
    head_csv += "\n";

    let data_csv = "";

    this.dataItems.forEach((item) => {
      item.forEach((item_column) => {
        data_csv += item_column + ",";
      });
      data_csv += "\n";
    });

    report_csv += head_csv;
    report_csv += data_csv;
    let filename: string =
      "downlowd" + Date.parse(new Date().toString()) + ".csv";
    let blob = new Blob([report_csv], { type: "text/csv;charset=utf-8" });
    if (window.navigator.msSaveOrOpenBlob) {
      window.navigator.msSaveOrOpenBlob(blob, filename);
    } else {
      let urlCreator = window.URL;
      let downUrl = urlCreator.createObjectURL(blob);
      let a = document.createElement("a");
      a.download = filename;
      a.href = downUrl;
      a.click();
      a.remove();
    }
  }

  getTables() {
    let that = this;
    axios
      .get("/tables")
      .then((res) => {
        that.options.hintOptions.tables = res.data.data;
        that.tables = res.data.data;

        // 初始化编辑器实例，传入需要被实例化的文本域对象和默认配置
        that.coder = CodeMirror.fromTextArea(
          that.$refs.textarea as HTMLTextAreaElement,
          that.options
        );
        // 编辑器赋值
        that.coder.setValue(that.value || that.code);

        // 支持双向绑定
        that.coder.on("change", (coder) => {
          that.code = coder.getValue();

          if (that.$emit) {
            that.$emit("input", that.code);
          }
        });

        return res.data.data;
      })
      .catch((err) => {
        alert(err.message);
      });
  }

  // 初始化 编辑器
  _initialize() {
    this.getTables();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
.head {
  display: flex;
  width: 100%;
  height: 50px;
  justify-content: flex-end;
  border-collapse: collapse;

  .eve {
    display: flex;
    width: 230px;
    height: 50px;
    justify-content: space-between;

    .exc {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100px;
      height: 30px;
      border-radius: 3px;
      background-color: bisque;

      div:hover {
        background: #e44f50;
      }
    }

    .fom {
      width: 100px;
      height: 30px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 3px;
      background-color: burlywood;
    }

    .exc:hover {
      background: #e44f50;
      cursor: pointer;
    }

    .fom:hover {
      background: #e44f50;
      cursor: pointer;
    }
  }
}

.in-coder-panel {
  flex-grow: 1;
  display: flex;
  position: relative;
  width: 100%;
  height: auto;
  justify-content: space-between;

  .tables {
    width: 20%;
    height: 100%;
    background-color: bisque;
    border-radius: 2px;
    text-align: left;
    .table {
        padding: 10px;
        border: 1px solid #fff;
        span {
            font-size: 20px;
        }
        .table_item {
            padding: 5px 10px;
        }
    }
  }

  .right {
    width: 78%;
    min-height: 820px;
    display: flex;
    flex-direction: column;

    textarea {
      width: 100%;
      height: 60%;
    }

    .CodeMirror-scroll {
        height: 300px;
    }

    .CodeMirror {
      width: 100%;
      text-align: left !important;

      .CodeMirror-code {
        line-height: 19px;
      }
    }

    .data {
      width: calc(100% - 2px);
      height: 40%;
      border: 1px solid cadetblue;
      display: flex;
      flex-direction: column;
      overflow: hidden;
      overflow-y: scroll;
      overflow-x: scroll;

      .data-head {
        display: flex;
        flex-direction: row;
        width: 100%;

        .data-head-column {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 80px;
          padding: 8px 30px;
          border-bottom: 1px solid cadetblue;
          border-right: 1px solid cadetblue;
          overflow: hidden; /*超出部分隐藏*/
          white-space: nowrap; /*不换行*/
          text-overflow: ellipsis; /*超出部分文字以...显示*/
        }

        .data-head-column-last {
          display: flex;
          justify-content: center;
          align-items: center;
          padding: 8px 30px;
          width: 80px;
          border-bottom: 1px solid cadetblue;
          overflow: hidden; /*超出部分隐藏*/
          white-space: nowrap; /*不换行*/
          text-overflow: ellipsis; /*超出部分文字以...显示*/
        }
      }

      .data-item {
        display: flex;
        flex-direction: row;
        width: 100%;

        .data-item-column {
          display: flex;
          justify-content: center;
          align-items: center;
          padding: 8px 30px;
          width: 80px;
          border-bottom: 1px solid cadetblue;
          border-right: 1px solid cadetblue;
          overflow: hidden; /*超出部分隐藏*/
          white-space: nowrap; /*不换行*/
          text-overflow: ellipsis; /*超出部分文字以...显示*/
        }

        .data-item-column-last {
          display: flex;
          justify-content: center;
          align-items: center;
          padding: 8px 30px;
          width: 80px;
          border-bottom: 1px solid cadetblue;
          overflow: hidden; /*超出部分隐藏*/
          white-space: nowrap; /*不换行*/
          text-overflow: ellipsis; /*超出部分文字以...显示*/
        }
      }
    }
  }
}

.footer {
  padding-top: 20px;
  display: flex;
  width: 100%;
  height: 50px;
  justify-content: flex-end;

  div:hover {
    background: #e44f50;
  }

  .exc {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
    height: 35px;
    border-radius: 3px;
    background-color: bisque;
  }

  .exc:hover {
    background: #e44f50;
    cursor: pointer;
  }
}
</style>
