<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="图片地址" prop="imgUrl">
      <el-input v-model="dataForm.imgUrl" placeholder="图片地址"></el-input>
    </el-form-item>
    <el-form-item label="'跳转的链接地址'" prop="skipUrl">
      <el-input v-model="dataForm.skipUrl" placeholder="'跳转的链接地址'"></el-input>
    </el-form-item>
    <el-form-item label="’排序id‘" prop="sortId">
      <el-input v-model="dataForm.sortId" placeholder="’排序id‘"></el-input>
    </el-form-item>
    <el-form-item label="'是否开启',1是0否" prop="isOpen">
      <el-input v-model="dataForm.isOpen" placeholder="'是否开启',1是0否"></el-input>
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
          imgUrl: '',
          skipUrl: '',
          sortId: '',
          isOpen: ''
        },
        dataRule: {
          imgUrl: [
            { required: true, message: '图片地址不能为空', trigger: 'blur' }
          ],
          skipUrl: [
            { required: true, message: '跳转的链接地址不能为空', trigger: 'blur' }
          ],
          sortId: [
            { required: true, message: '排序id不能为空', trigger: 'blur' }
          ],
          isOpen: [
            { required: true, message: '是否开启,1是0否不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//homeswiper/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.imgUrl = data.homeSwiper.imgUrl
                this.dataForm.skipUrl = data.homeSwiper.skipUrl
                this.dataForm.sortId = data.homeSwiper.sortId
                this.dataForm.isOpen = data.homeSwiper.isOpen
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
              url: this.$http.adornUrl(`//homeswiper/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'imgUrl': this.dataForm.imgUrl,
                'skipUrl': this.dataForm.skipUrl,
                'sortId': this.dataForm.sortId,
                'isOpen': this.dataForm.isOpen
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
