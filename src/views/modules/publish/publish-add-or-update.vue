<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="用户名" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户名"></el-input>
    </el-form-item>
    <el-form-item label="浏览数" prop="viewCount">
      <el-input v-model="dataForm.viewCount" placeholder="浏览数"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="内容" prop="content">
      <el-input v-model="dataForm.content" placeholder="内容"></el-input>
    </el-form-item>
    <el-form-item label="是否置顶（1是，0否）" prop="isZhiDing">
      <el-input v-model="dataForm.isZhiDing" placeholder="是否置顶（1是，0否）"></el-input>
    </el-form-item>
    <el-form-item label="类型ID" prop="typeId">
      <el-input v-model="dataForm.typeId" placeholder="类型ID"></el-input>
    </el-form-item>
    <el-form-item label="纬度" prop="latitude">
      <el-input v-model="dataForm.latitude" placeholder="纬度"></el-input>
    </el-form-item>
    <el-form-item label="发布地址" prop="address">
      <el-input v-model="dataForm.address" placeholder="发布地址"></el-input>
    </el-form-item>
    <el-form-item label="经度" prop="longitude">
      <el-input v-model="dataForm.longitude" placeholder="经度"></el-input>
    </el-form-item>
    <el-form-item label="图片地址" prop="imgs">
      <el-input v-model="dataForm.imgs" placeholder="图片地址"></el-input>
    </el-form-item>
    <el-form-item label="手机号" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="手机号"></el-input>
    </el-form-item>
    <el-form-item label="付费方式（1是积分，2是现款）" prop="pay">
      <el-input v-model="dataForm.pay" placeholder="付费方式（1是积分，2是现款）"></el-input>
    </el-form-item>
    <el-form-item label="发布价格" prop="price">
      <el-input v-model="dataForm.price" placeholder="发布价格"></el-input>
    </el-form-item>
    <el-form-item label="" prop="category">
      <el-input v-model="dataForm.category" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="'更新时间'" prop="updateTime">
      <el-input v-model="dataForm.updateTime" placeholder="'更新时间'"></el-input>
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
          viewCount: '',
          createTime: '',
          content: '',
          isZhiDing: '',
          typeId: '',
          latitude: '',
          address: '',
          longitude: '',
          imgs: '',
          phone: '',
          pay: '',
          price: '',
          category: '',
          updateTime: ''
        },
        dataRule: {
          userId: [
            { required: true, message: '用户名不能为空', trigger: 'blur' }
          ],
          viewCount: [
            { required: true, message: '浏览数不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          content: [
            { required: true, message: '内容不能为空', trigger: 'blur' }
          ],
          isZhiDing: [
            { required: true, message: '是否置顶（1是，0否）不能为空', trigger: 'blur' }
          ],
          typeId: [
            { required: true, message: '类型ID不能为空', trigger: 'blur' }
          ],
          latitude: [
            { required: true, message: '纬度不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '发布地址不能为空', trigger: 'blur' }
          ],
          longitude: [
            { required: true, message: '经度不能为空', trigger: 'blur' }
          ],
          imgs: [
            { required: true, message: '图片地址不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '手机号不能为空', trigger: 'blur' }
          ],
          pay: [
            { required: true, message: '付费方式（1是积分，2是现款）不能为空', trigger: 'blur' }
          ],
          price: [
            { required: true, message: '发布价格不能为空', trigger: 'blur' }
          ],
          category: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          updateTime: [
            { required: true, message: '更新时间h不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//publish/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.publish.userId
                this.dataForm.viewCount = data.publish.viewCount
                this.dataForm.createTime = data.publish.createTime
                this.dataForm.content = data.publish.content
                this.dataForm.isZhiDing = data.publish.isZhiDing
                this.dataForm.typeId = data.publish.typeId
                this.dataForm.latitude = data.publish.latitude
                this.dataForm.address = data.publish.address
                this.dataForm.longitude = data.publish.longitude
                this.dataForm.imgs = data.publish.imgs
                this.dataForm.phone = data.publish.phone
                this.dataForm.pay = data.publish.pay
                this.dataForm.price = data.publish.price
                this.dataForm.category = data.publish.category
                this.dataForm.updateTime = data.publish.updateTime
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
              url: this.$http.adornUrl(`//publish/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'userId': this.dataForm.userId,
                'viewCount': this.dataForm.viewCount,
                'createTime': this.dataForm.createTime,
                'content': this.dataForm.content,
                'isZhiDing': this.dataForm.isZhiDing,
                'typeId': this.dataForm.typeId,
                'latitude': this.dataForm.latitude,
                'address': this.dataForm.address,
                'longitude': this.dataForm.longitude,
                'imgs': this.dataForm.imgs,
                'phone': this.dataForm.phone,
                'pay': this.dataForm.pay,
                'price': this.dataForm.price,
                'category': this.dataForm.category,
                'updateTime': this.dataForm.updateTime
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
