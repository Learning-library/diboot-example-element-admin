<template>
    <div class="app-container">
      <div class="table-page-search-wrapper">
      <el-form :inline="true" label-width="100px">
        <el-row :gutter="18">
                <el-col :md="8" :sm="24">
            <el-form-item label="任务表" style="width: 100%;">
              <el-select v-model="queryParam.bisTaskId" placeholder="请选择任务表" style="width: 100%;">
                <el-option
                  v-if="more.bisTaskKvList" 
                  v-for="(item, index) in more.bisTaskKvList"
                  :key="index"
                  :value="item.v"
                  :label="item.k"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="24">
            <el-form-item label="任务步骤名称" style="width: 100%;">
              <el-input v-model="queryParam.taskStepName" placeholder="" style="width: 100%;"/>
            </el-form-item>
          </el-col>
          <template v-if="advanced">
          <el-col :md="8" :sm="24">
            <el-form-item label="任务步骤开始时间" style="width: 100%;">
              <el-date-picker
                v-model="queryParam.taskStepStartTime"
                type="date"
                style="width: 100%;"
                value-format="yyyy-MM-dd">
              </el-date-picker>
          </el-form-item>
        </el-col>
          <el-col :md="8" :sm="24">
            <el-form-item label="任务步骤结束时间" style="width: 100%;">
              <el-date-picker
                v-model="queryParam.taskStepEndTime"
                type="date"
                style="width: 100%;"
                value-format="yyyy-MM-dd">
              </el-date-picker>
          </el-form-item>
        </el-col>
          </template>
          <el-col :md="!advanced && 8 || 24" :sm="24">
            <span class="table-page-search-submitButtons" :style="advanced && { float: 'right', overflow: 'hidden' } || {} ">
              <el-button v-waves type="primary" icon="el-icon-search" @click="onSearch">
                查询
              </el-button>
              <el-button  style="margin-left: 8px" type="info" icon="el-icon-refresh" @click="reset">
               重置
              </el-button>
              <el-link type="primary" :underline="false" @click="toggleAdvanced" style="margin-left: 8px">
                {{ advanced ? '收起' : '展开' }}
                <i :class="advanced ? 'el-icon-arrow-up' : 'el-icon-arrow-down'"/>
              </el-link>
            </span>
          </el-col>
        </el-row>
      </el-form>
    </div>
    <div class="table-operator">
          <el-button v-permission="['create']" style="margin-left: 10px;" type="primary" icon="el-icon-plus" @click="$refs.form.open(undefined)">
        新建
      </el-button>
    </div>
<el-table
      v-loading="loadingData"
      :data="list"
      element-loading-text="Loading"
      fit
      @sort-change="appendSorterParam"
      highlight-current-row
      row-key="id"
    >
      <el-table-column  align="center" label="任务步骤名称">
      	<template slot-scope="scope">
          <span>{{ scope.row.taskStepName }}</span>
        </template>
      </el-table-column>
      <el-table-column  align="center" label="任务步骤描述">
      	<template slot-scope="scope">
          <span>{{ scope.row.taskStepDesc }}</span>
        </template>
      </el-table-column>
      <el-table-column  align="center" label="任务步骤开始时间">
      	<template slot-scope="scope">
          <span>{{ scope.row.taskStepStartTime }}</span>
        </template>
      </el-table-column>
      <el-table-column  align="center" label="任务步骤结束时间">
      	<template slot-scope="scope">
          <span>{{ scope.row.taskStepEndTime }}</span>
        </template>
      </el-table-column>
      <el-table-column  align="center" label="任务步骤单价">
      	<template slot-scope="scope">
          <span>{{ scope.row.taskStepPrice }}</span>
        </template>
      </el-table-column>
      <el-table-column  align="center" label="创建时间">
      	<template slot-scope="scope">
          <span>{{ scope.row.createTime }}</span>
        </template>
      </el-table-column>
      <el-table-column  align="center" label="更新时间">
      	<template slot-scope="scope">
          <span>{{ scope.row.updateTime }}</span>
        </template>
      </el-table-column>
      <el-table-column  align="center" label="任务表">
      	<template slot-scope="scope">
          <span>{{ scope.row.bisTaskTaskName }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="230" class-name="small-padding fixed-width">
      	<template slot-scope="{row}">
        	<el-button
            v-permission="['detail']"
            type="text"
            @click="$refs.detail.open(row.id)"
          >
            详情
          </el-button>
        	<span
            v-permission="['detail']"
            v-permission-again="['update', 'delete']"
          >
            <el-divider
              direction="vertical"
            />
          </span>
        	<el-dropdown
            v-permission="['update', 'delete']"
            @command="command => menuCommand(command, row)"
          >
          	<el-button type="text">
              更多<i class="el-icon-arrow-down el-icon--right" />
            </el-button>
            <el-dropdown-menu slot="dropdown">
							<el-dropdown-item
                v-permission="['update']"
                command="update"
                icon="el-icon-edit"
              >
                编辑
              </el-dropdown-item>
							<el-dropdown-item
                v-permission="['delete']"
                command="delete"
                icon="el-icon-delete"
              >
                删除
              </el-dropdown-item>
            </el-dropdown-menu>
        </el-dropdown>
				</template>
      </el-table-column>
    </el-table>
			<diboot-form ref="form" @complete="getList"></diboot-form>
      <diboot-detail ref="detail"></diboot-detail>
    </div>
</template>

<script>
import list from '@/components/diboot/mixins/list'
import waves from '@/directive/waves'
import dibootForm from './form'
import dibootDetail from './detail'

export default {
  name: 'BisTaskStepList',
  directives: { waves },
  components: {
    dibootForm,
    dibootDetail
  },
  mixins: [list],
  data() {
    return {
      baseApi: '/bisTaskStep',
      getListFromMixin: true,
           attachMoreList: [
        {
          type: 'T',
          target: 'bis_task',
          key: 'task_name',
          value: 'id'
        }
      ],

    }
  }
}
</script>
<style lang="scss" scoped>
</style>
