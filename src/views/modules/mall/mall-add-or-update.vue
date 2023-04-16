<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="积分" prop="score">
      <el-input v-model="dataForm.score" placeholder="积分"></el-input>
    </el-form-item>
    <el-form-item label="价格" prop="price">
      <el-input v-model="dataForm.price" placeholder="价格"></el-input>
    </el-form-item>
    <el-form-item label="商品图片" prop="image">
      <el-input v-model="dataForm.image" placeholder="商品图片"></el-input>
    </el-form-item>
    <el-form-item label="商品描述" prop="detail">
      <el-input v-model="dataForm.detail" placeholder="商品描述"></el-input>
    </el-form-item>
    <el-form-item label="原价" prop="oldPrice">
      <el-input v-model="dataForm.oldPrice" placeholder="原价"></el-input>
    </el-form-item>
    <el-form-item label="商品名" prop="name">
      <el-input v-model="dataForm.name" placeholder="商品名"></el-input>
    </el-form-item>
    <el-form-item label="运费" prop="freight">
      <el-input v-model="dataForm.freight" placeholder="运费"></el-input>
    </el-form-item>
    <el-form-item label="库存" prop="count">
      <el-input v-model="dataForm.count" placeholder="库存"></el-input>
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
          score: '',
          price: '',
          image: '',
          detail: '',
          oldPrice: '',
          name: '',
          freight: '',
          count: ''
        },
        dataRule: {
          score: [
            { required: true, message: '积分不能为空', trigger: 'blur' }
          ],
          price: [
            { required: true, message: '价格不能为空', trigger: 'blur' }
          ],
          image: [
            { required: true, message: '商品图片不能为空', trigger: 'blur' }
          ],
          detail: [
            { required: true, message: '商品描述不能为空', trigger: 'blur' }
          ],
          oldPrice: [
            { required: true, message: '原价不能为空', trigger: 'blur' }
          ],
          name: [
            { required: true, message: '商品名不能为空', trigger: 'blur' }
          ],
          freight: [
            { required: true, message: '运费不能为空', trigger: 'blur' }
          ],
          count: [
            { required: true, message: '库存不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//mall/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.score = data.mall.score
                this.dataForm.price = data.mall.price
                this.dataForm.image = data.mall.image
                this.dataForm.detail = data.mall.detail
                this.dataForm.oldPrice = data.mall.oldPrice
                this.dataForm.name = data.mall.name
                this.dataForm.freight = data.mall.freight
                this.dataForm.count = data.mall.count
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
              url: this.$http.adornUrl(`//mall/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'score': this.dataForm.score,
                'price': this.dataForm.price,
                'image': this.dataForm.image,
                'detail': this.dataForm.detail,
                'oldPrice': this.dataForm.oldPrice,
                'name': this.dataForm.name,
                'freight': this.dataForm.freight,
                'count': this.dataForm.count
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
