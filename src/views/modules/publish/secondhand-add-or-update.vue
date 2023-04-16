<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="'创建时间'" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="'创建时间'"></el-input>
    </el-form-item>
    <el-form-item label="更新时间" prop="updateTime">
      <el-input v-model="dataForm.updateTime" placeholder="更新时间"></el-input>
    </el-form-item>
    <el-form-item label="类别" prop="type">
      <el-input v-model="dataForm.type" placeholder="类别"></el-input>
    </el-form-item>
    <el-form-item label="细节" prop="detail">
      <el-input v-model="dataForm.detail" placeholder="细节"></el-input>
    </el-form-item>
    <el-form-item label="图片" prop="image">
      <el-input v-model="dataForm.image" placeholder="图片"></el-input>
    </el-form-item>
    <el-form-item label="状态" prop="status">
      <el-input v-model="dataForm.status" placeholder="状态"></el-input>
    </el-form-item>
    <el-form-item label="新旧程度" prop="degree">
      <el-input v-model="dataForm.degree" placeholder="新旧程度"></el-input>
    </el-form-item>
    <el-form-item label="浏览数" prop="viewCount">
      <el-input v-model="dataForm.viewCount" placeholder="浏览数"></el-input>
    </el-form-item>
    <el-form-item label="地址" prop="address">
      <el-input v-model="dataForm.address" placeholder="地址"></el-input>
    </el-form-item>
    <el-form-item label="现价" prop="price">
      <el-input v-model="dataForm.price" placeholder="现价"></el-input>
    </el-form-item>
    <el-form-item label="原价" prop="oldPrice">
      <el-input v-model="dataForm.oldPrice" placeholder="原价"></el-input>
    </el-form-item>
    <el-form-item label="卖家用户id" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="卖家用户id"></el-input>
    </el-form-item>
    <el-form-item label="'物品名称'" prop="itemName">
      <el-input v-model="dataForm.itemName" placeholder="'物品名称'"></el-input>
    </el-form-item>
    <el-form-item label="'手机号'" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="'手机号'"></el-input>
    </el-form-item>
    <el-form-item label="‘0是邮寄’，‘1’是自提" prop="transactionMode">
      <el-input v-model="dataForm.transactionMode" placeholder="‘0是邮寄’，‘1’是自提"></el-input>
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
          updateTime: '',
          type: '',
          detail: '',
          image: '',
          status: '',
          degree: '',
          viewCount: '',
          address: '',
          price: '',
          oldPrice: '',
          userId: '',
          itemName: '',
          phone: '',
          transactionMode: ''
        },
        dataRule: {
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          updateTime: [
            { required: true, message: '更新时间不能为空', trigger: 'blur' }
          ],
          type: [
            { required: true, message: '类别不能为空', trigger: 'blur' }
          ],
          detail: [
            { required: true, message: '细节不能为空', trigger: 'blur' }
          ],
          image: [
            { required: true, message: '图片不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ],
          degree: [
            { required: true, message: '新旧程度不能为空', trigger: 'blur' }
          ],
          viewCount: [
            { required: true, message: '浏览数不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '地址不能为空', trigger: 'blur' }
          ],
          price: [
            { required: true, message: '现价不能为空', trigger: 'blur' }
          ],
          oldPrice: [
            { required: true, message: '原价不能为空', trigger: 'blur' }
          ],
          userId: [
            { required: true, message: '卖家用户id不能为空', trigger: 'blur' }
          ],
          itemName: [
            { required: true, message: '物品名称不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '手机号不能为空', trigger: 'blur' }
          ],
          transactionMode: [
            { required: true, message: '‘0是邮寄’，‘1’是自提不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//secondhand/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.createTime = data.secondHand.createTime
                this.dataForm.updateTime = data.secondHand.updateTime
                this.dataForm.type = data.secondHand.type
                this.dataForm.detail = data.secondHand.detail
                this.dataForm.image = data.secondHand.image
                this.dataForm.status = data.secondHand.status
                this.dataForm.degree = data.secondHand.degree
                this.dataForm.viewCount = data.secondHand.viewCount
                this.dataForm.address = data.secondHand.address
                this.dataForm.price = data.secondHand.price
                this.dataForm.oldPrice = data.secondHand.oldPrice
                this.dataForm.userId = data.secondHand.userId
                this.dataForm.itemName = data.secondHand.itemName
                this.dataForm.phone = data.secondHand.phone
                this.dataForm.transactionMode = data.secondHand.transactionMode
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
              url: this.$http.adornUrl(`//secondhand/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'createTime': this.dataForm.createTime,
                'updateTime': this.dataForm.updateTime,
                'type': this.dataForm.type,
                'detail': this.dataForm.detail,
                'image': this.dataForm.image,
                'status': this.dataForm.status,
                'degree': this.dataForm.degree,
                'viewCount': this.dataForm.viewCount,
                'address': this.dataForm.address,
                'price': this.dataForm.price,
                'oldPrice': this.dataForm.oldPrice,
                'userId': this.dataForm.userId,
                'itemName': this.dataForm.itemName,
                'phone': this.dataForm.phone,
                'transactionMode': this.dataForm.transactionMode
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
