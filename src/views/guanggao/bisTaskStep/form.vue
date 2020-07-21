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
        label="任务表"
        prop="bisTaskId"
        :rules="{
          required: true, message: '任务表不能为空', trigger: 'blur', whitespace: true
        }"
      >
        <el-select
          placeholder="请选择任务表"
          v-model="form.bisTaskId"
          style="width: 100%;"
        >
          <el-option
            v-if="more.bisTaskKvList"
            v-for="(item, index) in more.bisTaskKvList"
            :key="index"
            :value="`${item.v}`"
            :label="item.k"
          >
          </el-option>
        </el-select>
      </el-form-item>
    </el-col>
    <el-col :span="24">
      <el-form-item
        label="任务步骤名称"
        prop="taskStepName"
        :rules="{
          required: true, message: '任务步骤名称不能为空', trigger: 'blur', whitespace: true
        }"
      >
        <el-input
          placeholder="请输入任务步骤名称"
          v-model="form.taskStepName"
        />
      </el-form-item>
    </el-col>
    <el-col :span="24">
      <el-form-item
        label="任务步骤描述"
      >
        <el-input
          placeholder="请输入任务步骤描述"
          v-model="form.taskStepDesc"
        />
      </el-form-item>
    </el-col>
    <el-col :span="24">
      <el-form-item
        label="任务步骤开始时间"
      >
        <el-date-picker
          v-model="form.taskStepStartTime"
          type="datetime"
          style="width: 100%;"
        />
      </el-form-item>
    </el-col>
    <el-col :span="24">
      <el-form-item
        label="任务步骤结束时间"
      >
        <el-date-picker
          v-model="form.taskStepEndTime"
          type="datetime"
          style="width: 100%;"
        />
      </el-form-item>
    </el-col>
    <el-col :span="24">
      <el-form-item
        label="任务步骤单价"
      >
      <el-input-number
        v-model="form.taskStepPrice"
        controls-position="right"
        placeholder="请输入任务步骤单价"
        style="width: 100%;"
      />
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
  name: 'BisTaskStepForm',
  components: {

  },
  mixins: [form],
  data() {
    return {
      baseApi: '/bisTaskStep',
      attachMoreList: [
        {
          type: 'T',
          target: 'bis_task',
          key: 'task_name',
          value: 'id'
        }
      ]
    }
  }
}
</script>
<style lang="scss" scoped>
</style>
