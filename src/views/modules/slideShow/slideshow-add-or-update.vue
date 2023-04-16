<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="是否启用" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="是否启用"></el-input>
    </el-form-item>
    <el-form-item label="排序id" prop="sortId">
      <el-input v-model="dataForm.sortId" placeholder="排序id"></el-input>
    </el-form-item>
    <el-form-item label="图片地址" prop="imageUrl">
      <el-input v-model="dataForm.imageUrl" placeholder="图片地址"></el-input>
    </el-form-item>
    <el-form-item label="跳转的链接地址" prop="skipUrl">
      <el-input v-model="dataForm.skipUrl" placeholder="跳转的链接地址"></el-input>
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
          userId: '',
          sortId: '',
          imageUrl: '',
          skipUrl: ''
        },
        dataRule: {
          userId: [
            { required: true, message: '是否启用不能为空', trigger: 'blur' }
          ],
          sortId: [
            { required: true, message: '排序id不能为空', trigger: 'blur' }
          ],
          imageUrl: [
            { required: true, message: '图片地址不能为空', trigger: 'blur' }
          ],
          skipUrl: [
            { required: true, message: '跳转的链接地址不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//slideshow/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.slideShow.userId
                this.dataForm.sortId = data.slideShow.sortId
                this.dataForm.imageUrl = data.slideShow.imageUrl
                this.dataForm.skipUrl = data.slideShow.skipUrl
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
              url: this.$http.adornUrl(`//slideshow/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'userId': this.dataForm.userId,
                'sortId': this.dataForm.sortId,
                'imageUrl': this.dataForm.imageUrl,
                'skipUrl': this.dataForm.skipUrl
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
