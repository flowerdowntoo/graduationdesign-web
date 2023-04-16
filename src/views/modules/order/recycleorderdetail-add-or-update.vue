<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="分类ID" prop="classificationId">
      <el-input v-model="dataForm.classificationId" placeholder="分类ID"></el-input>
    </el-form-item>
    <el-form-item label="估计重量" prop="estimateWeight">
      <el-input v-model="dataForm.estimateWeight" placeholder="估计重量"></el-input>
    </el-form-item>
    <el-form-item label="0：无图片上传 1：有图片上传" prop="hasPhoto">
      <el-input v-model="dataForm.hasPhoto" placeholder="0：无图片上传 1：有图片上传"></el-input>
    </el-form-item>
    <el-form-item label="图片地址" prop="detailPhotoUrls">
      <el-input v-model="dataForm.detailPhotoUrls" placeholder="图片地址"></el-input>
    </el-form-item>
    <el-form-item label="实际价格" prop="actualAmount">
      <el-input v-model="dataForm.actualAmount" placeholder="实际价格"></el-input>
    </el-form-item>
    <el-form-item label="订单号" prop="recycleOrderId">
      <el-input v-model="dataForm.recycleOrderId" placeholder="订单号"></el-input>
    </el-form-item>
    <el-form-item label="分类名" prop="classificationName">
      <el-input v-model="dataForm.classificationName" placeholder="分类名"></el-input>
    </el-form-item>
    <el-form-item label="实际重量" prop="actualWeight">
      <el-input v-model="dataForm.actualWeight" placeholder="实际重量"></el-input>
    </el-form-item>
    <el-form-item label="'回收员ID'" prop="recycleUserId">
      <el-input v-model="dataForm.recycleUserId" placeholder="'回收员ID'"></el-input>
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
          classificationId: '',
          estimateWeight: '',
          hasPhoto: '',
          detailPhotoUrls: '',
          actualAmount: '',
          recycleOrderId: '',
          classificationName: '',
          actualWeight: '',
          recycleUserId: ''
        },
        dataRule: {
          classificationId: [
            { required: true, message: '分类ID不能为空', trigger: 'blur' }
          ],
          estimateWeight: [
            { required: true, message: '估计重量不能为空', trigger: 'blur' }
          ],
          hasPhoto: [
            { required: true, message: '0：无图片上传 1：有图片上传不能为空', trigger: 'blur' }
          ],
          detailPhotoUrls: [
            { required: true, message: '图片地址不能为空', trigger: 'blur' }
          ],
          actualAmount: [
            { required: true, message: '实际价格不能为空', trigger: 'blur' }
          ],
          recycleOrderId: [
            { required: true, message: '订单号不能为空', trigger: 'blur' }
          ],
          classificationName: [
            { required: true, message: '分类名不能为空', trigger: 'blur' }
          ],
          actualWeight: [
            { required: true, message: '实际重量不能为空', trigger: 'blur' }
          ],
          recycleUserId: [
            { required: true, message: '回收员ID不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//recycleorderdetail/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.classificationId = data.recycleOrderDetail.classificationId
                this.dataForm.estimateWeight = data.recycleOrderDetail.estimateWeight
                this.dataForm.hasPhoto = data.recycleOrderDetail.hasPhoto
                this.dataForm.detailPhotoUrls = data.recycleOrderDetail.detailPhotoUrls
                this.dataForm.actualAmount = data.recycleOrderDetail.actualAmount
                this.dataForm.recycleOrderId = data.recycleOrderDetail.recycleOrderId
                this.dataForm.classificationName = data.recycleOrderDetail.classificationName
                this.dataForm.actualWeight = data.recycleOrderDetail.actualWeight
                this.dataForm.recycleUserId = data.recycleOrderDetail.recycleUserId
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
              url: this.$http.adornUrl(`//recycleorderdetail/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'classificationId': this.dataForm.classificationId,
                'estimateWeight': this.dataForm.estimateWeight,
                'hasPhoto': this.dataForm.hasPhoto,
                'detailPhotoUrls': this.dataForm.detailPhotoUrls,
                'actualAmount': this.dataForm.actualAmount,
                'recycleOrderId': this.dataForm.recycleOrderId,
                'classificationName': this.dataForm.classificationName,
                'actualWeight': this.dataForm.actualWeight,
                'recycleUserId': this.dataForm.recycleUserId
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
