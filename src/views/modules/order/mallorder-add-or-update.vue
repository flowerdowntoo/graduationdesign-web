<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="‘创建时间’" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="‘创建时间’"></el-input>
    </el-form-item>
    <el-form-item label="‘用户ID’" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="‘用户ID’"></el-input>
    </el-form-item>
    <el-form-item label="‘商品ID’" prop="goodsId">
      <el-input v-model="dataForm.goodsId" placeholder="‘商品ID’"></el-input>
    </el-form-item>
    <el-form-item label="‘积分’" prop="score">
      <el-input v-model="dataForm.score" placeholder="‘积分’"></el-input>
    </el-form-item>
    <el-form-item label="‘订单状态’" prop="status">
      <el-input v-model="dataForm.status" placeholder="‘订单状态’"></el-input>
    </el-form-item>
    <el-form-item label="‘更新时间’" prop="updateTime">
      <el-input v-model="dataForm.updateTime" placeholder="‘更新时间’"></el-input>
    </el-form-item>
    <el-form-item label="送达地址" prop="address">
      <el-input v-model="dataForm.address" placeholder="送达地址"></el-input>
    </el-form-item>
    <el-form-item label="‘订单号’" prop="orderId">
      <el-input v-model="dataForm.orderId" placeholder="‘订单号’"></el-input>
    </el-form-item>
    <el-form-item label="" prop="guserId">
      <el-input v-model="dataForm.guserId" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="'0'是商城，‘1’是二手，‘2’是废品" prop="orderType">
      <el-input v-model="dataForm.orderType" placeholder="'0'是商城，‘1’是二手，‘2’是废品"></el-input>
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
          createTime: '',
          userId: '',
          goodsId: '',
          score: '',
          status: '',
          updateTime: '',
          address: '',
          orderId: '',
          guserId: '',
          orderType: ''
        },
        dataRule: {
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          userId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          goodsId: [
            { required: true, message: '商品ID不能为空', trigger: 'blur' }
          ],
          score: [
            { required: true, message: '积分不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '订单状态不能为空', trigger: 'blur' }
          ],
          updateTime: [
            { required: true, message: '更新时间不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '送达地址不能为空', trigger: 'blur' }
          ],
          orderId: [
            { required: true, message: '订单号不能为空', trigger: 'blur' }
          ],
          guserId: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          orderType: [
            { required: true, message: '0是商城，1是二手，2是废品', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//mallorder/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.createTime = data.mallOrder.createTime
                this.dataForm.userId = data.mallOrder.userId
                this.dataForm.goodsId = data.mallOrder.goodsId
                this.dataForm.score = data.mallOrder.score
                this.dataForm.status = data.mallOrder.status
                this.dataForm.updateTime = data.mallOrder.updateTime
                this.dataForm.address = data.mallOrder.address
                this.dataForm.orderId = data.mallOrder.orderId
                this.dataForm.guserId = data.mallOrder.guserId
                this.dataForm.orderType = data.mallOrder.orderType
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
              url: this.$http.adornUrl(`//mallorder/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'createTime': this.dataForm.createTime,
                'userId': this.dataForm.userId,
                'goodsId': this.dataForm.goodsId,
                'score': this.dataForm.score,
                'status': this.dataForm.status,
                'updateTime': this.dataForm.updateTime,
                'address': this.dataForm.address,
                'orderId': this.dataForm.orderId,
                'guserId': this.dataForm.guserId,
                'orderType': this.dataForm.orderType
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
