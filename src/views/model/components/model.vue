<template>
  <div>
    <el-row :gutter="20">
      <el-col :span="6">
        <div class>
          模型名:
          <el-select
            v-model="listDataIndex"
            clearable
            filterable
            placeholder="请选择"
            @clear="cleartmp"
            @change="changeIndex"
          >
            <el-option
              v-for="(item, index) in listData"
              :key="item.id"
              :label="item.name"
              :value="index"
            />
          </el-select>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
        <div class>
          描述:
          <el-tag type="info">公式的详细描述:{{ tmp.desc }}</el-tag>
        </div>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="24">
        <div class>公式显示区:{{ tmp.formula }}</div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <div class>
          系数:
          <el-table :data="tmp.factor" height="250" border style="width: 100%">
            <el-table-column prop="key" label="系数" min-width="1" />
            <el-table-column prop="value" label="预定义值" min-width="1" />
          </el-table>
        </div>
      </el-col>
      <el-col :span="12">
        <div class>
          变量:
          <!-- https://codesandbox.io/s/2pv1o4wxxr?file=/src/App.vue -->
          <el-tag v-for="tag in tmp.variable" :key="tag" type="success">{{
            tag
          }}</el-tag>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tmp: {
        id: '',
        name: '',
        formula: '',
        desc: '',
        factor: [],
        variable: []
      },
      listData: [
        {
          id: '1',
          name: 'IPAT模型',
          formula: 'I=P*A*T',
          desc:
            'I代表人类活动对环境的影响，P代表人口，A代表人均财富，T代表技术水平。影响碳排放的因素包括人口规模、人均GDP和技术水平。',
          factor: [
            { key: 'P', value: 1 },
            { key: 'A', value: 2 },
            { key: 'T', value: 3 }
          ],
          variable: ['P', 'A', 'T']
        },
        {
          id: '2',
          name: '公式2',
          formula: 'y=ax+b',
          desc: '这是一个公式的描述',
          factor: [
            { key: 'key21', value: 1 },
            { key: 'key22', value: 2 },
            { key: 'key23', value: 3 },
            { key: 'key24', value: 4 }
          ],
          variable: ['x2', 'y2', 'z2']
        },
        {
          id: '3',
          name: '公式3',
          formula: 'y=ax+b',
          desc: '这是一个公式的描述',
          factor: [
            { key: 'key31', value: 1 },
            { key: 'key32', value: 2 },
            { key: 'key33', value: 3 },
            { key: 'key44', value: 4 }
          ],
          variable: ['x3', 'y3', 'z3']
        }
      ],
      listDataIndex: ''
    }
  },
  methods: {
    cleartmp() {
      this.tmp = {
        id: '',
        name: '',
        formula: '',
        desc: '',
        factor: [],
        variable: []
      }
    },
    changeIndex(index) {
      this.tmp = this.listData[index]
      // 这里提供一个事件给父组件，告知当前选中的模型的信息
      this.$emit('change', index)
    }
  }
}
</script>
<style scoped>
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.el-tag + .el-tag {
  margin-left: 10px;
}
</style>
