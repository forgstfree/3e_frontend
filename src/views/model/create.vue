<template>
  <div class="app-container">
    <el-row :gutter="20">
      <el-col :span="4">
        <div class="">公式名:</div>
      </el-col>
      <el-col :span="20">
        <div class="">
          <el-input type="textarea" :autosize="{ minRows: 1, maxRows: 2}" placeholder="请输入内容" v-model="nameFormula">
          </el-input>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="4">
        <div class="">公式:</div>
      </el-col>
      <el-col :span="20">
        <div class="">
          <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 4}" placeholder="请输入内容，支持Latax语法。以$$开始，以$$结束。" v-model="formula">
          </el-input>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="4">
        <div class="">格式化公式:</div>
      </el-col>
      <el-col :span="20">
        <div class="">
          <vue-mathjax :formula="formula"></vue-mathjax>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="4">
        <div class="">公式描述:</div>
      </el-col>
      <el-col :span="20">
        <div class="">
          <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 4}" placeholder="请输入内容" v-model="desc">
          </el-input>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="4">
        <div class="">公式备注:</div>
      </el-col>
      <el-col :span="20">
        <div class="">
          <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 4}" placeholder="请输入内容" v-model="remake">
          </el-input>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <div>系数区</div>
        <div>
          <el-table :data="coefficientData" style="width: 100%">
            <el-table-column prop="key" label="名称" width="180">
            </el-table-column>
            <el-table-column prop="value" label="预定义值">
            </el-table-column>
          </el-table>
          <el-button type="primary" style="{margin: 10px 50px 20px;}" @click="dialogVisible = true">新增</el-button>
        </div>
      </el-col>
      <el-col :span="12">
        <div>变量区</div>
        <div>
          <el-tag :key="tag" v-for="tag in dynamicTags" closable :disable-transitions="false" @close="handleClose(tag)">
            {{tag}}
          </el-tag>
          <el-input class="input-new-tag" v-if="inputVisible" v-model="inputValue" ref="saveTagInput" size="small"
            @keyup.enter.native="handleInputConfirm" @blur="handleInputConfirm">
          </el-input>
          <el-button v-else class="button-new-tag" size="small" @click="showInput">+ 新变量</el-button>
        </div>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="6" :offset="20">
        <div>
          <el-button type="success" @click="create">创建</el-button>
          <el-button type="danger" @click="clean">清除</el-button>
        </div>
      </el-col>
    </el-row>
    <div>
      <el-dialog title="新增系数" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
        <div>
          <el-row :gutter="20">
            <el-col :span="6">
              <div class="">参数名称：</div>
            </el-col>
            <el-col :span="18">
              <div class="">
                <el-input v-model="coefficient.key" placeholder="请输入参数名称"></el-input>
              </div>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="6">
              <div class="">预定义系数值：</div>
            </el-col>
            <el-col :span="18">
              <div class="">
                <el-input v-model="coefficient.value" placeholder="请输入参数预定义值"></el-input>
              </div>
            </el-col>
          </el-row>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="handleNewCoe">确 定</el-button>
        </span>
      </el-dialog>
    </div>

  </div>
</template>

<script>
import { VueMathjax } from 'vue-mathjax'
import { createModel } from '@/api/modelmanage'
export default {
  components: {
    'vue-mathjax': VueMathjax,
  },
  name: 'HelloWorld',
  data() {
    return {
      nameFormula: '',
      formula: '$$x = {-b \\pm \\sqrt{b^2-4ac} \\over 2a}.$$',
      desc: '',
      remake: '',
      dynamicTags: [],
      inputVisible: false,
      inputValue: '',
      coefficientData: [],
      dialogVisible: false,
      coefficient: {
        key: '',
        value: '',
      },
    }
  },
  methods: {
    handleNewCoe() {
      this.dialogVisible = false
      const tmp = JSON.parse(JSON.stringify(this.coefficient))
      this.coefficientData.push(tmp)
      this.coefficient = {
        key: '',
        value: '',
      }
    },
    handleClose(done) {
      this.$confirm('确认关闭？')
        .then((_) => {
        //   const tmp = this.coefficient
        //   this.coefficientData.push(tmp)
        })
        .catch((_) => {})
    },

    create() {
      const tmp = {
        name: this.nameFormula,
        expression: this.formula,
        desc: this.desc,
        remake: this.remake,
        coefficient: JSON.stringify(this.coefficientData),
        variable: this.dynamicTags.toString() || '[]',
      }
      console.log(tmp)
      createModel(tmp).then((response) => {
        console.log(response)
      })
    },
    clean() {
      this.nameFormula = ''
      this.formula = ''
      this.desc = ''
      this.remark = ''
      this.coefficientData = []
      this.dynamicTags = []
    },
    handleClose(tag) {
      this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1)
    },

    showInput() {
      this.inputVisible = true
      this.$nextTick((_) => {
        this.$refs.saveTagInput.$refs.input.focus()
      })
    },

    handleInputConfirm() {
      let inputValue = this.inputValue
      if (inputValue) {
        this.dynamicTags.push(inputValue)
      }
      this.inputVisible = false
      this.inputValue = ''
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
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
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
.el-tag + .el-tag {
  margin-left: 10px;
}
.button-new-tag {
  margin-left: 10px;
  height: 32px;
  line-height: 30px;
  padding-top: 0;
  padding-bottom: 0;
}
.input-new-tag {
  width: 90px;
  margin-left: 10px;
  vertical-align: bottom;
}
</style>
