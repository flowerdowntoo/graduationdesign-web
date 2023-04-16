<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="预约日期" prop="appointDate">
      <el-input v-model="dataForm.appointDate" placeholder="预约日期"></el-input>
    </el-form-item>
    <el-form-item label="预约时间" prop="appointTime">
      <el-input v-model="dataForm.appointTime" placeholder="预约时间"></el-input>
    </el-form-item>
    <el-form-item label="上门地址" prop="addressId">
      <el-input v-model="dataForm.addressId" placeholder="上门地址"></el-input>
    </el-form-item>
    <el-form-item label="回收类型（0是上门回收，1是公益捐赠）" prop="publicGive">
      <el-input v-model="dataForm.publicGive" placeholder="回收类型（0是上门回收，1是公益捐赠）"></el-input>
    </el-form-item>
    <el-form-item label="用户ID" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="更新时间" prop="updateTime">
      <el-input v-model="dataForm.updateTime" placeholder="更新时间"></el-input>
    </el-form-item>
    <el-form-item label="1是待回收 2是待确认 3已完成" prop="status">
      <el-input v-model="dataForm.status" placeholder="1是待回收 2是待确认 3已完成"></el-input>
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
          appointDate: '',
          appointTime: '',
          addressId: '',
          publicGive: '',
          userId: '',
          createTime: '',
          updateTime: '',
          status: ''
        },
        dataRule: {
          appointDate: [
            { required: true, message: '预约日期不能为空', trigger: 'blur' }
          ],
          appointTime: [
            { required: true, message: '预约时间不能为空', trigger: 'blur' }
          ],
          addressId: [
            { required: true, message: '上门地址不能为空', trigger: 'blur' }
          ],
          publicGive: [
            { required: true, message: '回收类型（0是上门回收，1是公益捐赠）不能为空', trigger: 'blur' }
          ],
          userId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          updateTime: [
            { required: true, message: '更新时间不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '1是待回收 2是待确认 3已完成不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//recycleorder/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.appointDate = data.recycleOrder.appointDate
                this.dataForm.appointTime = data.recycleOrder.appointTime
                this.dataForm.addressId = data.recycleOrder.addressId
                this.dataForm.publicGive = data.recycleOrder.publicGive
                this.dataForm.userId = data.recycleOrder.userId
                this.dataForm.createTime = data.recycleOrder.createTime
                this.dataForm.updateTime = data.recycleOrder.updateTime
                this.dataForm.status = data.recycleOrder.status
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
              url: this.$http.adornUrl(`//recycleorder/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'appointDate': this.dataForm.appointDate,
                'appointTime': this.dataForm.appointTime,
                'addressId': this.dataForm.addressId,
                'publicGive': this.dataForm.publicGive,
                'userId': this.dataForm.userId,
                'createTime': this.dataForm.createTime,
                'updateTime': this.dataForm.updateTime,
                'status': this.dataForm.status
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
