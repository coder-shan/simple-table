/**
* 列表组件:
1.固定表头,固定列
2.按列排序,筛选过滤
3.树形数据(支持异步)
4.拖拽列宽,拖拽列顺序(下次打开时记忆)
5.选择显示列(只看部分列)
*/
<template>
  <div class="table_container">
    <div class="btn_group">
      <el-tooltip class="item" effect="dark" content="筛选列" placement="bottom">
        <el-button icon="el-icon-menu" @click="dialogVisible = true"></el-button>
      </el-tooltip>
      <el-select v-model="condition" placeholder="请选择筛选条件">
        <el-option v-for="(item,index) in columns" :key="`option_${index}`" :label="item.label"
                   :value="item.prop"></el-option>
      </el-select>
      <el-input placeholder="请输入搜索内容" v-model="searchText" clearable>
      </el-input>
    </div>
    <div class="table_content">
      <el-table id="applyTable" :lazy="isLazy" :load="load" :data="data" :height='height' @header-dragend='surverWidth'
                :empty-text="empty_text" style="width: 100%" row-key="id" :tree-props="treeProps" border ref="table">
        <el-table-column v-for="(item,index) in columnList" :key="Math.random()" :prop="item.prop"
                         :label="columnNames[index]"
                         :width="item.width || ''" :fixed="item.isFixed || false" :sortable="item.isSort || false"
                         v-if="checkItems[index].isChecked">
        </el-table-column>
      </el-table>
    </div>
    <el-dialog title="筛选列" :visible.sync="dialogVisible" width="30%">
      <el-checkbox v-for="(item,index) in checkList" :key="index" v-model="item.isChecked">{{ item.label }}
      </el-checkbox>
      <span slot="footer" class="dialog-footer">
				<el-button @click="dialogVisible = false">取 消</el-button>
				<el-button type="primary" @click="filterData">确 定</el-button>
			</span>
    </el-dialog>
  </div>
  </div>
</template>

<script>
  import Sortable from 'sortablejs'

  export default {
    /**
     * tableData:
     */
    props: {
      tableData: Array,
      columns: Array,
      rowKey: {
        type: String,
        default: ''
      },
      treeProps: {
        type: Object,
        default: () => {
          return {
            children: 'children',
            hasChildren: 'hasChildren'
          }
        }
      },
      height: {
        type: Number,
        default: '400'
      },
      empty_text: {
        type: String,
        default: '暂无数据'
      },
      isLazy: {
        type: Boolean,
        default: false
      },
      load: ''
    },
    created() {
      this.init()
    },
    mounted() {
      this.columnDrop()
      this.rowDrop()
    },
    watch: {
      searchText() {
        if (this.condition) {
          this.data = this.tableData.filter((item) => {
            return (item[this.condition].indexOf(this.searchText) !== -1);
          })
        } else {
          this.$message.warning('请选择搜索条件')
        }
      }
    },
    data() {
      return {
        dialogVisible: false,
        checkList: [],
        checkItems: [],
        data: [],
        columnList: [],
        columnNames: [],
        searchText: '',
        condition: ''
      }
    },
    methods: {
      init() {
        this.columns.forEach(item => {
          this.checkList.push({
            label: item.label,
            isChecked: true
          })
        })
        if (localStorage.getItem('columnList')) {
          this.columnList = JSON.parse(localStorage.getItem('columnList'))
          this.columnNames = JSON.parse(localStorage.getItem('columnList')).map(item => item.label)
        } else {
          this.columnList = JSON.parse(JSON.stringify(this.columns))
          this.columnNames = this.columns.map(item => item.label)
        }
        if (localStorage.getItem('applyTableColWidths')) {
          let widths = JSON.parse(localStorage.getItem('applyTableColWidths'))
          this.columnList.forEach((item, index) => {
            item.width = widths[index]
          })
        }
        if (localStorage.getItem('tableData')) {
          this.data = JSON.parse(localStorage.getItem('tableData'))
        } else {
          this.data = JSON.parse(JSON.stringify(this.tableData))
        }
        this.checkItems = JSON.parse(JSON.stringify(this.checkList))
      },
      filterData() {
        this.checkItems = JSON.parse(JSON.stringify(this.checkList))
        this.dialogVisible = false
      },
      // 列拖拽
      columnDrop() {
        const wrapperTr = document.querySelector('.el-table__header-wrapper tr');
        this.sortable = Sortable.create(wrapperTr, {
          animation: 180,
          delay: 0,
          onEnd: evt => {
            const oldItem = this.columnList[evt.oldIndex];
            this.columnList.splice(evt.oldIndex, 1);
            this.columnList.splice(evt.newIndex, 0, oldItem);
            this.columnNames = []
            this.columnNames = this.columnList.map(item => item.label)
            localStorage.setItem('columnList', JSON.stringify(this.columnList))
          }
        });
      },
      //行拖拽
      rowDrop() {
        const el = document.querySelectorAll('.el-table__body-wrapper > table > tbody')[0];
        let self = this;
        Sortable.create(el, {
        // 拖拽结束后的操作
          onEnd({newIndex, oldIndex}) {
            // 修改data中的数组，
            const targetRow = self.data.splice(oldIndex, 1)[0]
            self.data.splice(newIndex, 0, targetRow)
            localStorage.setItem('tableData', JSON.stringify(self.data))
          }
        });

      },
      //获取每列宽度
      surverWidth(newWidth, oldWidth, column, event) {
        setTimeout(() => {
          var applyTableColWidths = []
          var applyTable = document.getElementById('applyTable')
          var applyTableColgroup = applyTable.getElementsByTagName('colgroup')[0]
          var applyTableCol = applyTableColgroup.getElementsByTagName('col')
          for (var i = 0; i < applyTableCol.length; i++) {
            applyTableColWidths.push(applyTableCol[i].width)
          }
          localStorage.setItem('applyTableColWidths', JSON.stringify(applyTableColWidths))
        }, 100)
      }
    }
  }
</script>

<style scoped>
  .table_content {
    padding: 24px;
  }

  .btn_group {
    display: flex;
    padding: 20px;
  }
</style>
