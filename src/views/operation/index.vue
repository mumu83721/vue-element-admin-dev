<template>
  <div class="components-container">
    <el-select ref="select" v-model="value" placeholder="请选择" style="width:200px;display:inline-block;vertical-align: middle;margin-left:10px;margin-bottom:10px;">
      <el-option v-for="item in tabMapOptions" :key="item.value" :label="item.label" :value="item.value" />
    </el-select>
    <el-button type="primary" style="width:200px;display:inline-block;vertical-align: middle;margin-left:10px;margin-bottom:10px;" @click="dialogTableVisible = true">
      新增
    </el-button>
    <el-dialog v-el-drag-dialog :visible.sync="dialogTableVisible" title="新增酒店" @dragDialog="handleDrag">
      <!-- <el-select ref="select" v-model="value" placeholder="请选择">
        <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
      </el-select> -->
      <el-form ref="postForm" :model="postForm" :rules="rules" class="form-container">
        <el-form-item label="名称:" :label-width="formLabelWidth">
          <el-input v-model="postForm.name" autocomplete="off" />
        </el-form-item>
        <area-select v-model="selected" :data="pca" />
        <label class="label" for="address">地址:
          <input v-model="postForm.address" name="address">
        </label>
      </el-form>
      <el-button v-loading="loading" style="margin-left: 10px;" type="success" @click="submitForm">
        添加
      </el-button>
    </el-dialog>
    <el-table :data="list" border fit highlight-current-row style="width: 100%">
      <el-table-column
        v-loading="loading"
        align="center"
        label="酒店id"
        width="150"
        element-loading-text="请给我点时间！"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>

      <el-table-column width="110px" align="center" label="酒店名称">
        <template slot-scope="scope">
          <span>{{ scope.row.name }}</span>
        </template>
      </el-table-column>

      <el-table-column width="110px" align="center" label="酒店地址">
        <template slot-scope="scope">
          <span>{{ scope.row.address }}</span>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>

import { addHotel, getHotel } from '@/api/article'
import { pca, pcaa } from 'area-data'

const defaultForm = {
  address: '',
  name: ''
}

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
      formLabelWidth: '120px',
      postForm: Object.assign({}, defaultForm),
      dialogTableVisible: false,
      list: null,
      listQuery: {
        page: 1,
        limit: 5,
        type: this.type,
        sort: '+id'
      },
      loading: false,
      selected: [],
      pca: pca,
      pcaa: pcaa
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.loading = true
      this.$emit('create') // for test
      getHotel(this.listQuery).then(response => {
        this.list = response.rows
        console.log('!!!!!' + JSON.stringify(response))
        this.loading = false
      })
    },
    submitForm() {
      this.loading = true
      console.log(this.postForm)
      addHotel(this.postForm).then(response => {
        this.loading = false
        this.dialogTableVisible = false
        this.getList()
      })
    }
  }
}
</script>
