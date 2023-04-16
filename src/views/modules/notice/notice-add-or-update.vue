<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="'公告内容'" prop="content">
      <el-input v-model="dataForm.content" placeholder="'公告内容'"></el-input>
    </el-form-item>
    <el-form-item label="是否启用，0否1是" prop="isOpen">
      <el-input v-model="dataForm.isOpen" placeholder="是否启用，0否1是"></el-input>
    </el-form-item>
    <el-form-item label="'排序id'" prop="sortId">
      <el-input v-model="dataForm.sortId" placeholder="'排序id'"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          content: '',
          isOpen: '',
          sortId: ''
        },
        dataRule: {
          content: [
            { required: true, message: '公告内容不能为空', trigger: 'blur' }
          ],
          isOpen: [
            { required: true, message: '是否启用，0否1是', trigger: 'blur' }
          ],
          sortId: [
            { required: true, message: '排序id不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`//notice/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.content = data.notice.content
                this.dataForm.isOpen = data.notice.isOpen
                this.dataForm.sortId = data.notice.sortId
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`//notice/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'content': this.dataForm.content,
                'isOpen': this.dataForm.isOpen,
                'sortId': this.dataForm.sortId
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
