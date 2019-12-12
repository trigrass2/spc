<template>
  <div>
    <head-title/>
    <div class="wrap">
      <div class="btn-tool">
        <btn-tool :buttons="btnList"/>
      </div>
      <div class="tempLayout">
        <add-echarts :grid-num="layoutType"/>
        <!-- layoutType -->
      </div>
      <!-- /btnTool -->
    </div>
    <!-- 弹窗 -->
    <el-dialog :visible.sync="dialogGridVisible" width="710px">
      <div slot="title" class="dialog-header">{{ dialogGridTitle }}</div>
      <el-form ref="gridForm" :model="gridForm" :rules="gridRules" class="gridFrom">
        <el-form-item label="布局类型" prop="type">
          <el-radio-group v-model="gridForm.type">
            <el-radio label="1">
              <choice-grid :grid-num="1"/>
            </el-radio>
            <el-radio label="2">
              <choice-grid :grid-num="2"/>
            </el-radio>
            <el-radio label="3">
              <choice-grid :grid-num="3"/>
            </el-radio>
            <el-radio label="4">
              <choice-grid :grid-num="4"/>
            </el-radio>
          </el-radio-group>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogGridVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogGridSubmit">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
import headTitle from '@/components/headTitle'
import btnTool from '@/components/btnTool'
import choiceGrid from '@/components/choiceGrid'
import addEcharts from '@/components/addEcharts'
export default {
  name: 'Template',
  components: {
    headTitle,
    btnTool,
    choiceGrid,
    addEcharts,
  },
  data () {
    return {
      // tool
      btnList: [
        {
          type: 'primary',
          icon: 'icontianjia',
          text: '添加布局',
          disabled: () => {
            return false
          },
          click: () => {
            this.gridAdd()
          },
        },
        {
          type: 'primary',
          icon: 'iconxiugai',
          text: '修改布局',
          disabled: () => {
            return true
          },
          click: () => {
            this.gridmodify()
          },
        },
        {
          type: 'success',
          icon: 'iconbaocun',
          text: '保存模板',
          disabled: false,
          click: () => {
            console.log('打印模板')
          },
        },
      ],
      // 选择布局
      gridForm: {
        type: '',
      },
      gridRules: {
        type: [
          {
            required: true,
            message: '请选择布局类型',
            trigger: 'change',
          },
        ],
      },
      layoutType: '3',
      dialogGridVisible: false,
      dialogGridTitle: '添加布局',
    }
  },
  computed: {

  },
  watch: {
    dialogGridVisible (val) {
      if (!val) {
        this.resetForm('gridForm')
      }
    },
  },
  methods: {
    // 选择布局
    gridAdd () {
      this.dialogGridVisible = true
      this.dialogGridTitle = '添加布局'
    },
    gridmodify () {
      this.dialogGridVisible = true
      this.dialogGridTitle = '修改布局'
    },
    dialogGridSubmit () {
      this.$refs.gridForm.validate(valid => {
        if (valid) {
          this.dialogGridVisible = false
          this.layoutType = this.gridForm.type
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    // 重置表单
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
  },
}
</script>
<style lang="scss" scoped>
.gridFrom {
  /deep/.el-form-item__label {
    line-height: 90px;
  }
  /deep/.el-radio-group {
    display: flex;
    flex-direction: row;
    .el-radio {
      margin-right: 40px;
      display: flex;
      flex-direction: row;
      align-items: center;
    }
  }
  /deep/.el-form-item__error {
    top: 105%;
    left: 100px;
  }
}
.btn-tool {
  border-bottom: 1px solid $line-color;
}
// 模板布局
.tempLayout {
  margin-top: 15px;
  padding: 10px;
  background: #eeecec;
  height: calc(100vh - 235px);
}
</style>