<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="真实姓名" prop="name">
      <el-input v-model="dataForm.name" placeholder="真实姓名"></el-input>
    </el-form-item>
    <el-form-item label="用户ID" prop="openid">
      <el-input v-model="dataForm.openid" placeholder="用户ID"></el-input>
    </el-form-item>
    <el-form-item label="头像" prop="avatarurl">
      <el-input v-model="dataForm.avatarurl" placeholder="头像"></el-input>
    </el-form-item>
    <el-form-item label="昵称" prop="nickname">
      <el-input v-model="dataForm.nickname" placeholder="昵称"></el-input>
    </el-form-item>
    <el-form-item label="1是用户 2是回收员" prop="userType">
      <el-input v-model="dataForm.userType" placeholder="1是用户 2是回收员"></el-input>
    </el-form-item>
    <el-form-item label="积分" prop="score">
      <el-input v-model="dataForm.score" placeholder="积分"></el-input>
    </el-form-item>
    <el-form-item label="0女，1男" prop="sex">
      <el-input v-model="dataForm.sex" placeholder="0女，1男"></el-input>
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
          openid: '',
          avatarurl: '',
          nickname: '',
          userType: '',
          score: '',
          sex: ''
        },
        dataRule: {
          name: [
            { required: true, message: '真实姓名不能为空', trigger: 'blur' }
          ],
          openid: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          avatarurl: [
            { required: true, message: '头像不能为空', trigger: 'blur' }
          ],
          nickname: [
            { required: true, message: '昵称不能为空', trigger: 'blur' }
          ],
          userType: [
            { required: true, message: '1是用户 2是回收员不能为空', trigger: 'blur' }
          ],
          score: [
            { required: true, message: '积分不能为空', trigger: 'blur' }
          ],
          sex: [
            { required: true, message: '0女，1男不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//user/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.user.name
                this.dataForm.openid = data.user.openid
                this.dataForm.avatarurl = data.user.avatarurl
                this.dataForm.nickname = data.user.nickname
                this.dataForm.userType = data.user.userType
                this.dataForm.score = data.user.score
                this.dataForm.sex = data.user.sex
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
              url: this.$http.adornUrl(`//user/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'openid': this.dataForm.openid,
                'avatarurl': this.dataForm.avatarurl,
                'nickname': this.dataForm.nickname,
                'userType': this.dataForm.userType,
                'score': this.dataForm.score,
                'sex': this.dataForm.sex
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
