<template>
  <el-dialog
    :title="!dataForm.questionId ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="垃圾类型" prop="garbageType">
      <el-input v-model="dataForm.garbageType" placeholder="垃圾类型"></el-input>
    </el-form-item>
    <el-form-item label="垃圾名称" prop="garbageName">
      <el-input v-model="dataForm.garbageName" placeholder="垃圾名称"></el-input>
    </el-form-item>
    <el-form-item label="解析" prop="analysis">
      <el-input v-model="dataForm.analysis" placeholder="解析"></el-input>
    </el-form-item>
    <el-form-item label="备注" prop="remark">
      <el-input v-model="dataForm.remark" placeholder="备注"></el-input>
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
          questionId: 0,
          garbageType: '',
          garbageName: '',
          analysis: '',
          remark: ''
        },
        dataRule: {
          garbageType: [
            { required: true, message: '垃圾类型不能为空', trigger: 'blur' }
          ],
          garbageName: [
            { required: true, message: '垃圾名称不能为空', trigger: 'blur' }
          ],
          analysis: [
            { required: true, message: '解析不能为空', trigger: 'blur' }
          ],
          remark: [
            { required: true, message: '备注不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.questionId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.questionId) {
            this.$http({
              url: this.$http.adornUrl(`//questionbank/info/${this.dataForm.questionId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.garbageType = data.questionBank.garbageType
                this.dataForm.garbageName = data.questionBank.garbageName
                this.dataForm.analysis = data.questionBank.analysis
                this.dataForm.remark = data.questionBank.remark
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
              url: this.$http.adornUrl(`//questionbank/${!this.dataForm.questionId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'questionId': this.dataForm.questionId || undefined,
                'garbageType': this.dataForm.garbageType,
                'garbageName': this.dataForm.garbageName,
                'analysis': this.dataForm.analysis,
                'remark': this.dataForm.remark
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
