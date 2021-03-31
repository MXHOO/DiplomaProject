<template>
  <div class="rc-site">
    <div class="rc-site__cover">
      <img src="@/assets/show.png">
    </div>
    <div class="rc-site__describe">
      <p>作业ID:&nbsp; {{siteDetail.homework_id}}
        <i v-show="siteDetail.homework_id" type="copy" class="operator el-icon-copy-document"
          v-clipboard:copy="siteDetail.homework_id" v-clipboard:success="onCopy" v-clipboard:error="onError" />
      </p>
      <p>作业名称：{{ siteDetail.homework_name }}
        <i class="el-icon-edit" @click="editSite" />
      </p>
      <p>作业须知：{{ siteDetail.homework_notice }}</p>
    </div>
    <div class="rc-site__operator">
      <div>
        <span class="operator" @click="editSiteContent">
          <i class="el-icon-edit">编辑</i>
        </span>
      </div>
      <div>
        <el-dropdown trigger="click">
          <span class="operator">
            更多
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item @click="deleteHomework">
              <i class="el-icon-delete operator" style="font-size: 14px;"> 删除作业</i>
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </div>
    <div class="rc-site__status">
      <p v-if="siteDetail.status === 1" class="rc-site__status--published">已发布</p>
      <p v-if="siteDetail.status === 0 || siteDetail.status === null" class="rc-site__status--unpublished">未发布</p>
    </div>
    <!-- 编辑作业 -->
    <el-dialog title="编辑作业" :visible="editVisible" @cancel="clearValid">
      <el-form :model="form" :label-col="{ span: 4 }" ref="form" :wrapper-col="{ span: 19 }" :rules="rules">
        <el-form-item label="作业名称" prop="title">
          <el-input v-model.trim="form.homework_name" placeholder="请输入作业名称" />
        </el-form-item>
        <el-form-item label="作业描述" prop="description">
          <el-input v-model.trim="form.homework_notice" placeholder="请输入作业描述"></el-input>
        </el-form-item>
      </el-form>
      <template slot="footer">
        <el-button type="primary" @click="confirmModal">确认</el-button>
        <el-button @click="clearValid">取消</el-button>
      </template>
    </el-dialog>
  </div>
</template>
<script>
// import { previewWorkUrl } from '@/services/work.service'
export default {
  props: {
    siteDetail: {
      type: Object,
      default: () => { }
    }
  },
  data () {
    return {
      editVisible: false,
      previewVisible: false,
      form: {},
      rules: {
        pageMode: [{ required: true, message: '作业类型不能为空', trigger: 'change' }],
        title: [{ required: true, message: '作业标题不能为空', trigger: 'blur' }],
        description: [{ required: true, message: '作业描述不能为空', trigger: 'blur' }]
      },
      pageModeList: [
        { label: 'H5', value: 'h5' },
        { label: 'PC', value: 'pc' }
      ],
      protocol: window.location.protocol
      // previewUrl: previewWorkUrl
    }
  },
  methods: {
    editSite () {
      this.form = Object.assign({}, this.siteDetail)
      this.editVisible = true
    },
    clearValid () {
      this.editVisible = false
      this.$refs.form && this.$refs.form.clearValidate()
    },
    confirmModal () {
      this.$refs.form.validate((valid) => {
        if (valid) {
          this.$emit('saveSite', this.form)
          this.clearValid()
        }
      })
    },
    onCopy () {
      this.$message.success('内容复制成功', 0.5)
    },
    onError () {
      this.$message.error('内容复制失败,请重新复制！', 0.5)
    },
    editSiteContent () {
      // this.$router.push(`/editor/${this.siteDetail.activityId}`)
    },
    // 删除作业
    deleteHomework () {
      console.log('删除作业')
      // const self = this
      // this.$confirm('确认删除该作业吗？', '提示').then(_ => {
      //   self.$emit('deleteHomework', self.siteDetail.activityId)
      // }).catch(_ => {})
    }
  }
}
</script>
<style lang="scss" scoped>
.rc-site {
  width: 230px;
  font-size: 14px;
  margin: 15px;
  border: #eeeeee 1px solid;
  border-radius: 4px;
  position: relative;

  &__cover {
    overflow: hidden;
    display: flex;
    justify-content: center;

    img {
    width: 50%;
    height: 80%;
    border-top-left-radius: 4px;
    border-top-right-radius: 4px;
    margin: 10px 0;
  }
  }

  &__describe {
    padding: 5px 10px;

    p {
      margin-top: 2px;
      margin-bottom: 8px;
    }
  }

  &__operator {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 34px;
    padding: 10px;
    background: #f9f9f9;

    div {
      text-align: center;
      flex: 1;
    }
  }

  .operator {
    &:hover {
      cursor: pointer;
    }
  }

  &__preview {
    .phone {
      width: 320px;
      height: 568px;
      margin: 0 auto;
      border: 10px solid #8d8d8d;
      border-radius: 10px;
      box-sizing: content-box;
    }
  }

  &__status {
    position: absolute;
    right: 10px;
    top: 10px;

    p {
      padding: 3px;
      border-radius: 4px;
    }

    &--published {
      background: #63ce81;
    }

    &--unpublished {
      background: #f95d5d;
    }
  }
}
</style>