<template>
  <el-dialog :append-to-body="true" :visible.sync="dialog" :title="isAdd ? '新增' : '编辑'" width="500px">
    <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
      <el-form-item label="审批标题">
        <el-input v-model="form.title" style="width: 370px;"/>
      </el-form-item>
      <el-form-item label="审批用户ID">
        <el-input v-model="form.userId" style="width: 370px;"/>
      </el-form-item>
      <el-form-item label="有效期结束时间">
        <el-input v-model="form.validEndTime" style="width: 370px;"/>
      </el-form-item>
      <el-form-item label="下一步审批用户ID">
        <el-input v-model="form.nextExuserId" style="width: 370px;"/>
      </el-form-item>
      <el-form-item label="当前状态">
        <el-input v-model="form.currentState" style="width: 370px;"/>
      </el-form-item>
      <el-form-item label="链接地址">
        <el-input v-model="form.linkAddress" style="width: 370px;"/>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button type="text" @click="cancel">取消</el-button>
      <el-button :loading="loading" type="primary" @click="doSubmit">确认</el-button>
    </div>
  </el-dialog>
</template>

<script>
import { add, edit } from '@/api/approveItem'
export default {
  props: {
    isAdd: {
      type: Boolean,
      required: true
    },
    sup_this: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      loading: false, dialog: false,
      form: {
        id: '',
        title: '',
        userId: '',
        validEndTime: '',
        nextExuserId: '',
        currentState: '',
        linkAddress: ''
      }
    }
  },
  methods: {
    cancel() {
      this.resetForm()
    },
    doSubmit() {
      this.loading = true
      if (this.isAdd) {
        this.doAdd()
      } else this.doEdit()
    },
    doAdd() {
      add(this.form).then(res => {
        this.resetForm()
        this.$notify({
          title: '添加成功',
          type: 'success',
          duration: 2500
        })
        this.loading = false
        this.$parent.$parent.init()
      }).catch(err => {
        this.loading = false
        console.log(err.response.data.message)
      })
    },
    doEdit() {
      edit(this.form).then(res => {
        this.resetForm()
        this.$notify({
          title: '修改成功',
          type: 'success',
          duration: 2500
        })
        this.loading = false
        this.sup_this.init()
      }).catch(err => {
        this.loading = false
        console.log(err.response.data.message)
      })
    },
    resetForm() {
      this.dialog = false
      this.$refs['form'].resetFields()
      this.form = {
        id: '',
        title: '',
        userId: '',
        validEndTime: '',
        nextExuserId: '',
        currentState: '',
        linkAddress: ''
      }
    }
  }
}
</script>

<style scoped>

</style>
