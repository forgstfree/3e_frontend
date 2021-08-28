<template>
  <el-table :data="tableData" stripe style="width: 100%">
    <el-table-column prop="variable" align="center" label="变量" width="180" />
    <el-table-column prop="data" label="字段" min-width="1">
      <template slot-scope="scope">
        <el-cascader v-model="scope.row.data" :options="dataList" />
        <!-- <el-select v-model="scope.row.data" placeholder="请选择" size="mini"> -->
        <!-- <el-option v-for="item in dataList" :key="item.tableID" :label="item.label" :value="item.value"> -->
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        {
          variable: 'x',
          data: []
        },
        {
          variable: 'y',
          data: {
          }
        },
        {
          variable: 'z',
          data: {
          }
        }
      ],
      dataList: [
        {
          value: '1',
          label: '表1',
          children: [
            {
              value: '11',
              label: '列11'
            },
            {
              value: '12',
              label: '列12'
            },
            {
              value: '13',
              label: '列13'
            },
            {
              value: '14',
              label: '列14'
            }
          ]
        },
        {
          value: '2',
          label: '表2',
          children: [
            {
              value: '21',
              label: '列21'
            },
            {
              value: '22',
              label: '列22'
            },
            {
              value: '23',
              label: '列23'
            },
            {
              value: '24',
              label: '列24'
            }
          ]
        }
      ]
    }
  }
}
</script>
