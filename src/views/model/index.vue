<template>
  <div class="app-container">
    <div class="build-container">
      <el-steps :active="active" finish-status="success">
        <el-step title="步骤 1" description="选择模型">1</el-step>
        <el-step title="步骤 2" description="选择数据">2</el-step>
        <el-step title="步骤 3" description="字段映射">3</el-step>
        <el-step title="步骤 4" description="分析">4</el-step>
      </el-steps>

      <el-button :disabled="active===1" style="margin-top: 12px;" @click="last">上一步</el-button>
      <el-button type="primary" style="margin-top: 12px;margin-bottom: 12px;" @click="next">下一步</el-button>
      <div v-show="active===1" class="step1">
        <Model3E ref="model" @change="handleModel" />
      </div>
      <div v-show="active===2" class="step2">
        <Datafields ref="datafields" />
      </div>
      <div v-show="active===3" class="step3">
        <Mapper ref="mapper" />
      </div>
      <div v-show="active===4" class="step4">
        <ShowResult ref="showresult" />
        <!-- <el-button type="primary" @click="buildJson">分析</el-button> -->
        <!-- <el-button type="primary" @click="handleJobTemplateSelectDrawer">{{ jobTemplate ? jobTemplate : "2.选择模板" }}</el-button> -->
        <!-- <el-button type="info" @click="handleCopy(inputData,$event)">复制json</el-button> -->
        <!-- (步骤：构建->选择模板->下一步) -->
      </div>

    </div>
  </div>
</template>

<script>
import clip from '@/utils/clipboard'
import Model3E from './components/model'
import Datafields from './components/datafields'
import Mapper from './components/mapper'
import ShowResult from './components/showresult'

export default {
  name: 'JsonBuild',
  components: { Model3E, Datafields, Mapper, ShowResult },
  data() {
    return {
      active: 1,
      list: null,
      currentRow: null,
      listLoading: true,
      total: 0,
      triggerNextTimes: '',
      registerNode: []
    }
  },
  created() {
    // this.getJdbcDs()
  },
  methods: {
    handleModel() {

    },
    next() {
      //   const fromColumnList = this.$refs.reader.getData().columns
      //   const toColumnsList = this.$refs.writer.getData().columns
      // const fromTableName = this.$refs.reader.getData().tableName
      // 第一步 reader 判断是否已选字段
      if (this.active === 1) {
        // 实现第一步骤读取的表和字段直接带到第二步骤
        // this.$refs.writer.sendTableNameAndColumns(fromTableName, fromColumnList)
        // 取子组件的数据
        // console.info(this.$refs.reader.getData())
        this.active++
      } else {
        if (this.active === 2) {
        }
        if (this.active === 4) {
        } else {
          this.active++
        }
      }
    },
    last() {
      if (this.active > 1) {
        this.active--
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.step4 {
  position: relative;
  width: 100%;
  /* height: calc(100vh - 84px); */
  height: 884px;
}
</style>
