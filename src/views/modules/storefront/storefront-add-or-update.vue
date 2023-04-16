<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="店铺名" prop="shopName">
      <el-input v-model="dataForm.shopName" placeholder="店铺名"></el-input>
    </el-form-item>
    <el-form-item label="浏览量" prop="viewCount">
      <el-input v-model="dataForm.viewCount" placeholder="浏览量"></el-input>
    </el-form-item>
    <el-form-item label="评分" prop="rate">
      <el-input v-model="dataForm.rate" placeholder="评分"></el-input>
    </el-form-item>
    <el-form-item label="地址" prop="address">
      <el-input v-model="dataForm.address" placeholder="地址"></el-input>
    </el-form-item>
    <el-form-item label="联系电话" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="联系电话"></el-input>
    </el-form-item>
    <el-form-item label="地址细节" prop="addressDetail">
      <el-input v-model="dataForm.addressDetail" placeholder="地址细节"></el-input>
    </el-form-item>
    <el-form-item label="营业开始时间" prop="businessStartTime">
      <el-input v-model="dataForm.businessStartTime" placeholder="营业开始时间"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="审核状态(0是待审核 1是审核通过 2是未通过)" prop="status">
      <el-input v-model="dataForm.status" placeholder="审核状态(0是待审核 1是审核通过 2是未通过)"></el-input>
    </el-form-item>
    <el-form-item label="租金" prop="price">
      <el-input v-model="dataForm.price" placeholder="租金"></el-input>
    </el-form-item>
    <el-form-item label="到期时间" prop="expirationTime">
      <el-input v-model="dataForm.expirationTime" placeholder="到期时间"></el-input>
    </el-form-item>
    <el-form-item label="用户ID" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID"></el-input>
    </el-form-item>
    <el-form-item label="回收类型" prop="typeId">
      <el-input v-model="dataForm.typeId" placeholder="回收类型"></el-input>
    </el-form-item>
    <el-form-item label="店铺介绍" prop="detail">
      <el-input v-model="dataForm.detail" placeholder="店铺介绍"></el-input>
    </el-form-item>
    <el-form-item label="营业结束时间" prop="businessEndTime">
      <el-input v-model="dataForm.businessEndTime" placeholder="营业结束时间"></el-input>
    </el-form-item>
    <el-form-item label="纬度" prop="latitude">
      <el-input v-model="dataForm.latitude" placeholder="纬度"></el-input>
    </el-form-item>
    <el-form-item label="经度" prop="longitude">
      <el-input v-model="dataForm.longitude" placeholder="经度"></el-input>
    </el-form-item>
    <el-form-item label="头像" prop="touXiang">
      <el-input v-model="dataForm.touXiang" placeholder="头像"></el-input>
    </el-form-item>
    <el-form-item label="封面" prop="fengMianImg">
      <el-input v-model="dataForm.fengMianImg" placeholder="封面"></el-input>
    </el-form-item>
    <el-form-item label="轮播图" prop="banner">
      <el-input v-model="dataForm.banner" placeholder="轮播图"></el-input>
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
          shopName: '',
          viewCount: '',
          rate: '',
          address: '',
          phone: '',
          addressDetail: '',
          businessStartTime: '',
          createTime: '',
          status: '',
          price: '',
          expirationTime: '',
          userId: '',
          typeId: '',
          detail: '',
          businessEndTime: '',
          latitude: '',
          longitude: '',
          touXiang: '',
          fengMianImg: '',
          banner: ''
        },
        dataRule: {
          shopName: [
            { required: true, message: '店铺名不能为空', trigger: 'blur' }
          ],
          viewCount: [
            { required: true, message: '浏览量不能为空', trigger: 'blur' }
          ],
          rate: [
            { required: true, message: '评分不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '地址不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '联系电话不能为空', trigger: 'blur' }
          ],
          addressDetail: [
            { required: true, message: '地址细节不能为空', trigger: 'blur' }
          ],
          businessStartTime: [
            { required: true, message: '营业开始时间不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '审核状态(0是待审核 1是审核通过 2是未通过)不能为空', trigger: 'blur' }
          ],
          price: [
            { required: true, message: '租金不能为空', trigger: 'blur' }
          ],
          expirationTime: [
            { required: true, message: '到期时间不能为空', trigger: 'blur' }
          ],
          userId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          typeId: [
            { required: true, message: '回收类型不能为空', trigger: 'blur' }
          ],
          detail: [
            { required: true, message: '店铺介绍不能为空', trigger: 'blur' }
          ],
          businessEndTime: [
            { required: true, message: '营业结束时间不能为空', trigger: 'blur' }
          ],
          latitude: [
            { required: true, message: '纬度不能为空', trigger: 'blur' }
          ],
          longitude: [
            { required: true, message: '经度不能为空', trigger: 'blur' }
          ],
          touXiang: [
            { required: true, message: '头像不能为空', trigger: 'blur' }
          ],
          fengMianImg: [
            { required: true, message: '封面不能为空', trigger: 'blur' }
          ],
          banner: [
            { required: true, message: '轮播图不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//storefront/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.shopName = data.storefront.shopName
                this.dataForm.viewCount = data.storefront.viewCount
                this.dataForm.rate = data.storefront.rate
                this.dataForm.address = data.storefront.address
                this.dataForm.phone = data.storefront.phone
                this.dataForm.addressDetail = data.storefront.addressDetail
                this.dataForm.businessStartTime = data.storefront.businessStartTime
                this.dataForm.createTime = data.storefront.createTime
                this.dataForm.status = data.storefront.status
                this.dataForm.price = data.storefront.price
                this.dataForm.expirationTime = data.storefront.expirationTime
                this.dataForm.userId = data.storefront.userId
                this.dataForm.typeId = data.storefront.typeId
                this.dataForm.detail = data.storefront.detail
                this.dataForm.businessEndTime = data.storefront.businessEndTime
                this.dataForm.latitude = data.storefront.latitude
                this.dataForm.longitude = data.storefront.longitude
                this.dataForm.touXiang = data.storefront.touXiang
                this.dataForm.fengMianImg = data.storefront.fengMianImg
                this.dataForm.banner = data.storefront.banner
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
              url: this.$http.adornUrl(`//storefront/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'shopName': this.dataForm.shopName,
                'viewCount': this.dataForm.viewCount,
                'rate': this.dataForm.rate,
                'address': this.dataForm.address,
                'phone': this.dataForm.phone,
                'addressDetail': this.dataForm.addressDetail,
                'businessStartTime': this.dataForm.businessStartTime,
                'createTime': this.dataForm.createTime,
                'status': this.dataForm.status,
                'price': this.dataForm.price,
                'expirationTime': this.dataForm.expirationTime,
                'userId': this.dataForm.userId,
                'typeId': this.dataForm.typeId,
                'detail': this.dataForm.detail,
                'businessEndTime': this.dataForm.businessEndTime,
                'latitude': this.dataForm.latitude,
                'longitude': this.dataForm.longitude,
                'touXiang': this.dataForm.touXiang,
                'fengMianImg': this.dataForm.fengMianImg,
                'banner': this.dataForm.banner
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
