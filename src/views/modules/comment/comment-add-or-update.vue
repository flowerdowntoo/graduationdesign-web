<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="评论内容" prop="content">
      <el-input v-model="dataForm.content" placeholder="评论内容"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createtime">
      <el-input v-model="dataForm.createtime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="用户id" prop="userid">
      <el-input v-model="dataForm.userid" placeholder="用户id"></el-input>
    </el-form-item>
    <el-form-item label="更新时间" prop="updatetime">
      <el-input v-model="dataForm.updatetime" placeholder="更新时间"></el-input>
    </el-form-item>
    <el-form-item label="点赞数" prop="commentlikecount">
      <el-input v-model="dataForm.commentlikecount" placeholder="点赞数"></el-input>
    </el-form-item>
    <el-form-item label="回复对象id" prop="tocommentid">
      <el-input v-model="dataForm.tocommentid" placeholder="回复对象id"></el-input>
    </el-form-item>
    <el-form-item label="所属发布id" prop="publishid">
      <el-input v-model="dataForm.publishid" placeholder="所属发布id"></el-input>
    </el-form-item>
    <el-form-item label="根评论id" prop="rootcommentid">
      <el-input v-model="dataForm.rootcommentid" placeholder="根评论id"></el-input>
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
          content: '',
          createtime: '',
          userid: '',
          updatetime: '',
          commentlikecount: '',
          tocommentid: '',
          publishid: '',
          rootcommentid: ''
        },
        dataRule: {
          content: [
            { required: true, message: '评论内容不能为空', trigger: 'blur' }
          ],
          createtime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          userid: [
            { required: true, message: '用户id不能为空', trigger: 'blur' }
          ],
          updatetime: [
            { required: true, message: '更新时间不能为空', trigger: 'blur' }
          ],
          commentlikecount: [
            { required: true, message: '点赞数不能为空', trigger: 'blur' }
          ],
          tocommentid: [
            { required: true, message: '回复对象id不能为空', trigger: 'blur' }
          ],
          publishid: [
            { required: true, message: '所属发布id不能为空', trigger: 'blur' }
          ],
          rootcommentid: [
            { required: true, message: '根评论id不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`//comment/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.content = data.comment.content
                this.dataForm.createtime = data.comment.createtime
                this.dataForm.userid = data.comment.userid
                this.dataForm.updatetime = data.comment.updatetime
                this.dataForm.commentlikecount = data.comment.commentlikecount
                this.dataForm.tocommentid = data.comment.tocommentid
                this.dataForm.publishid = data.comment.publishid
                this.dataForm.rootcommentid = data.comment.rootcommentid
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
              url: this.$http.adornUrl(`//comment/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'content': this.dataForm.content,
                'createtime': this.dataForm.createtime,
                'userid': this.dataForm.userid,
                'updatetime': this.dataForm.updatetime,
                'commentlikecount': this.dataForm.commentlikecount,
                'tocommentid': this.dataForm.tocommentid,
                'publishid': this.dataForm.publishid,
                'rootcommentid': this.dataForm.rootcommentid
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
