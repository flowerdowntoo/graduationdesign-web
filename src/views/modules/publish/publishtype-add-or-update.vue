<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="发布类型名" prop="name">
      <el-input v-model="dataForm.name" placeholder="发布类型名"></el-input>
    </el-form-item>
    <el-form-item label="图片" prop="icon">
      <el-input v-model="dataForm.icon" placeholder="图片"></el-input>
    </el-form-item>
    <el-form-item label="'0'是买，‘1’是卖" prop="isBuy">
      <el-input v-model="dataForm.isBuy" placeholder="'0'是买，‘1’是卖"></el-input>
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
          name: '',
          icon: '',
          isBuy: ''
        },
        dataRule: {
          name: [
            { required: true, message: '发布类型名不能为空', trigger: 'blur' }
          ],
          icon: [
            { required: true, message: '图片不能为空', trigger: 'blur' }
          ],
          isBuy: [
            { required: true, message: '0是买，1是卖不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//publishtype/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.publishType.name
                this.dataForm.icon = data.publishType.icon
                this.dataForm.isBuy = data.publishType.isBuy
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
              url: this.$http.adornUrl(`//publishtype/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'icon': this.dataForm.icon,
                'isBuy': this.dataForm.isBuy
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
