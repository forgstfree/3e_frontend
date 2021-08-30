<template>
  <div>
    <el-table
      ref="multipleTable"
      :data="listData"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55" />
      <el-table-column prop="name" label="表名" width="120" />
      <el-table-column prop="fields" label="字段" show-overflow-tooltip>
        <template slot-scope="scope">
          <!-- <span style="margin-left: 10px">{{ scope.row.date }}</span> -->
          <el-tag
            v-for="tag in scope.row.fields"
            :key="tag"
            type="success"
            :disable-transitions="false"
          >
            {{ tag }}
          </el-tag>
        </template>
      </el-table-column>
    </el-table>
    <div style="margin-top: 20px">
      <!-- <el-button @click="toggleSelection([tableData[1], tableData[2]])">切换第二、第三行的选中状态</el-button> -->
      <!-- <el-button @click="toggleSelection()">取消选择</el-button> -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      listData: [
        {
          name: '表1',
          fields: ['列1', '列2', '列3']
        },
        {
          name: '表2',
          fields: ['列1', '列2', '列3']
        },
        {
          name: '表3',
          fields: ['列1', '列2', '列3']
        }
      ],
      multipleSelection: []
    }
  },

  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val
    },
    fetchHeaderData() {
      // 获取数据列表和列字段
    },
    fetchSeletion() {

    }
  }
}
</script>
<style scoped>
.el-tag + .el-tag {
    margin-left: 10px;
  }
</style>
