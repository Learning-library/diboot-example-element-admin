<template>
    <el-dialog
      :visible.sync="state.visible"
      :fullscreen="fullscreen"
      :custom-class="!fullscreen ? 'custom-height': 'custom-fullscreen'"
      :show-close="false"
    >
        <el-row slot="title" type="flex">
            <el-col :span="20">{{ title }}</el-col>
            <el-col :span="4" style="text-align: right">
                <svg-icon
                  :icon-class="!fullscreen ? 'fullscreen': 'exit-fullscreen'"
                  style="cursor: pointer; margin-right: 10px"
                  @click="() => {fullscreen = !fullscreen}"
                />
                <i class="el-icon-close" style="cursor: pointer" @click="close" />
            </el-col>
        </el-row>
        <el-form ref="dataForm" :model="form" label-position="right" label-width="100px">
  <el-row :gutter="18">
    <el-col :span="24">
      <el-form-item
        label="任务名称"
        prop="taskName"
        :rules="{
          required: true, message: '任务名称不能为空', trigger: 'blur', whitespace: true
        }"
      >
        <el-input
          placeholder="请输入任务名称"
          v-model="form.taskName"
        />
      </el-form-item>
    </el-col>
    <el-col :span="24">
      <el-form-item
        label="任务描述"
      >
          <el-input
            type="textarea"
            placeholder="请输入任务描述"
            v-model="form.taskDesc"
          />
      </el-form-item>
    </el-col>
    <el-col :span="24">
      <el-form-item
        label="任务类型 "
      >
        <el-select
          placeholder="请选择任务类型 "
          v-model="form.taskType"
          style="width: 100%;"
        >
          <el-option
            v-if="more.taskTypeKvList"
            v-for="(item, index) in more.taskTypeKvList"
            :key="index"
            :value="item.v"
            :label="item.k"
          >
          </el-option>
        </el-select>
      </el-form-item>
    </el-col>
  </el-row>
</el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="close">
                取消
            </el-button>
            <el-button type="primary" :loading="state.confirmSubmit" :disabled="state.confirmSubmit" @click="onSubmit">
                确定
            </el-button>
        </div>
    </el-dialog>
</template>

<script>
import form from '@/components/diboot/mixins/form'

export default {
  name: 'BisTaskForm',
  components: {

  },
  mixins: [form],
  data() {
    return {
      baseApi: '/bisTask',
      attachMoreList: [
        {
          type: 'D',
          target: 'TASK_TYPE'
        }
      ]
    }
  }
}
</script>
<style lang="scss" scoped>
</style>
