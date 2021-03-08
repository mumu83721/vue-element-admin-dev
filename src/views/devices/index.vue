<template>
  <div class="components-container">
    <el-select ref="select" v-model="value" placeholder="请选择" style="width:200px;display:inline-block;vertical-align: middle;margin-left:10px;margin-bottom:10px;">
      <el-option v-for="item in tabMapOptions" :key="item.value" :label="item.label" :value="item.value" />
    </el-select>
    <el-table :data="list" border fit highlight-current-row style="width: 100%">
      <el-table-column
        v-loading="loading"
        align="center"
        label="地区"
        width="150px"
        element-loading-text="请给我点时间！"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.Hotel.province+scope.row.Hotel.city }}</span>
        </template>
      </el-table-column>

      <el-table-column width="200px" align="center" label="详细地址">
        <template slot-scope="scope">
          <span>{{ scope.row.Hotel.address }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" label="酒店名称">
        <template slot-scope="scope">
          <span>{{ scope.row.Hotel.name }}</span>
        </template>
      </el-table-column>

      <el-table-column width="110px" align="center" label="房间号">
        <template slot-scope="scope">
          <span>{{ scope.row.room }}</span>
        </template>
      </el-table-column>
      <el-table-column width="110px" align="center" label="设备id">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>

      <el-table-column width="110px" align="center" label="设备状态">
        <template slot-scope="scope">
          <span>{{ scope.row.status }}</span>
        </template>
      </el-table-column>

      <el-table-column width="110px" align="center" label="安装人">
        <template slot-scope="scope">
          <span>{{ scope.row.installman }}</span>
        </template>
      </el-table-column>

      <el-table-column width="110px" align="center" label="安装时间">
        <template slot-scope="scope">
          <span>{{ scope.row.created_at }}</span>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { fetchDeviceList } from '@/api/article'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  props: {
    type: {
      type: String,
      default: 'CN'
    }
  },
  data() {
    return {
      list: null,
      listQuery: {
        page: 0,
        size: 20,
        type: this.type,
        sort: '+id'
      },
      loading: false
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.loading = true
      this.$emit('create') // for test
      fetchDeviceList(this.listQuery).then(response => {
        this.list = response.rows
        console.log(this.list)
        this.loading = false
      })
    }
  }
}
</script>
