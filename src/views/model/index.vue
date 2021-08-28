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
        <Model3E ref="model" />
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
      listQuery: {
        current: 1,
        size: 10,
        jobGroup: 0,
        triggerStatus: -1,
        jobDesc: '',
        executorHandler: '',
        userId: 0
      },
      triggerNextTimes: '',
      registerNode: [],
      temp: {
        id: undefined,
        jobGroup: '',
        jobCron: '',
        jobDesc: '',
        executorRouteStrategy: '',
        executorBlockStrategy: '',
        childJobId: '',
        executorFailRetryCount: '',
        alarmEmail: '',
        executorTimeout: '',
        userId: 0,
        jobConfigId: '',
        executorHandler: 'executorJobHandler',
        glueType: 'BEAN',
        jobJson: '',
        executorParam: '',
        replaceParam: '',
        jvmParam: '',
        incStartTime: ''
      }
    }
  },
  created() {
    // this.getJdbcDs()
  },
  methods: {
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
    },
    // 构建json
    buildJson() {
      const readerData = this.$refs.reader.getData()
      const writeData = this.$refs.writer.getData()
      console.log(readerData)
      console.log(writeData)
      const readerColumns = this.$refs.mapper.getLColumns()
      const writerColumns = this.$refs.mapper.getRColumns()
      const hiveReader = {
        readerPath: readerData.path,
        readerDefaultFS: readerData.defaultFS,
        readerFileType: readerData.fileType,
        readerFieldDelimiter: readerData.fieldDelimiter,
        readerSkipHeader: readerData.skipHeader
      }
      const hiveWriter = {
        writerDefaultFS: writeData.defaultFS,
        writerFileType: writeData.fileType,
        writerPath: writeData.path,
        writerFileName: writeData.fileName,
        writeMode: writeData.writeMode,
        writeFieldDelimiter: writeData.fieldDelimiter
      }
      const hbaseReader = {
        readerMode: readerData.mode,
        readerMaxVersion: readerData.maxVersion,
        readerRange: readerData.range
      }
      const hbaseWriter = {
        writerMode: writeData.mode,
        writerRowkeyColumn: writeData.rowkeyColumn,
        writerVersionColumn: writeData.versionColumn,
        writeNullMode: writeData.nullMode
      }
      const obj = {
        readerDatasourceId: readerData.datasourceId,
        readerTables: [readerData.tableName],
        readerColumns: readerColumns,
        writerDatasourceId: writeData.datasourceId,
        writerTables: [writeData.tableName],
        writerColumns: writerColumns,
        hiveReader: hiveReader,
        hiveWriter: hiveWriter,
        hbaseReader: hbaseReader,
        hbaseWriter: hbaseWriter
      }
      // 调api
      //   dataxJsonApi.buildJobJson(obj).then(response => {
      //     this.configJson = JSON.parse(response)
      //   })
    },
    handleCopy(text, event) {
      clip(this.configJson, event)
      this.$message({
        message: 'copy success',
        type: 'success'
      })
    },
    handleJobTemplateSelectDrawer() {
      this.jobTemplateSelectDrawer = !this.jobTemplateSelectDrawer
      if (this.jobTemplateSelectDrawer) {
        this.fetchData()
        this.getExecutor()
      }
    },
    getReaderData() {
      return this.$refs.reader.getData()
    },
    fetchData() {
      this.listLoading = true
      //   jobTemplate.getList(this.listQuery).then(response => {
      //     const { content } = response
      //     this.total = content.recordsTotal
      //     this.list = content.data
      //     this.listLoading = false
      //   })
    },
    handleCurrentChange(val) {
      this.temp = Object.assign({}, val)
      this.temp.id = undefined
      this.temp.jobDesc = this.getReaderData().tableName
      this.$refs.jobTemplateSelectDrawer.closeDrawer()
      this.jobTemplate = val.id + '(' + val.jobDesc + ')'
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
