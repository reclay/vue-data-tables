<style lang='scss'>
  body {
    margin: 0;
    font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
  }

  .app-wrapper {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }

  $li-width: 20px;
  .el-custom-pagination{
    list-style: none;
    ul{
      padding: 0;
    }
    li{
      display: inline-block;
      width: $li-width;
      height: $li-width;
      background-color: red;
      border-radius: $li-width / 2;
      cursor: pointer;
      margin-left: 10px;
      &.active{
        background-color: black;
      }
    }
  }
</style>

<template lang="pug">
  .app-wrapper
    data-tables(
    :data='tableData',
    :tableProps='tableProps',
    :pagination-def='paginationDef',
    @selection-change='handleSelectChange'
      @row-click='handleRowClick'
        @sort-change='sortM')

      el-table-column(prop='date', label='date.', sortable='custom')
      el-table-column(prop='name', label='name.', sortable='custom')
      el-table-column(prop='num', label='num.', sortable='custom')

    //- div {{selection}}
</template>

<script>
  import DataTables from '@/index.js'

  export default {
    components: {DataTables},
    data() {
      return {
        tableData: [
          {
            date: '2016-05-02',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1518 弄',
            url: 'https://jsfiddle.net/api/post/library/pure//jsfiddle.net/api/post/library/pure',
            num: 100
          }, {
            date: '2016-05-04',
            name: '毛小虎',
            address: '上海市普陀区金沙江路 1517 弄',
            url: 'http://www.dltv.cn/vod/ds/ggpd/fzxtd/1471002888976.shtml?t=2016-8-12&id=14690',
            num: 37
          }, {
            date: '2016-05-01',
            name: '非小虎',
            address: '上海市普陀区金沙江路 1519 弄',
            url: '',
            num: 150
          }, {
            date: '2016-05-03',
            name: '张小虎',
            address: '上海市普陀区金沙江路 1516 弄',
            url: '',
            num: 300
          }, {
            date: '2016-05-02',
            name: '狗小虎',
            address: '上海市普陀区金沙江路 1518 弄',
            url: 'https://jsfiddle.net/api/post/library/pure//jsfiddle.net/api/post/library/pure',
            num: 100
          },
          {
            date: '2016-05-04',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1517 弄',
            url: 'http://www.dltv.cn/vod/ds/ggpd/fzxtd/1471002888976.shtml?t=2016-8-12&id=14690',
            num: 137
          }, {
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄',
            url: '',
            num: 150
          }, {
            date: '2016-05-03',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1516 弄',
            url: '',
            num: 300
          }
        ],
        tableProps: {
          rowClassName: 'test-class',
          border: false,
          stripe: false
        },
        customFilters: [{
          vals: ''
        }, {
          vals: []
        }],
        selection: {},
        actionsDef: {
          colProps: {
            span: 5
          },
          def: [{
            name: 'new',
            handler: () => {
              this.$message('new clicked')
            },
            buttonProps: {
              type: 'text'
            }
          }, {
            name: 'import',
            handler: () => {
              this.$message('import clicked')
            },
            icon: 'upload'
          }]
        },
        checkFilterDef: {
          props: 'state_code',
          def: [{
            'code': 'created',
            'name': 'Created'
          }, {
            'code': 'assigned',
            'name': 'Assigned'
          }, {
            'code': 'accepted',
            'name': 'Accepted'
          }, {
            'code': 'closed',
            'name': 'Closed'
          }, {
            'code': 'cancelled',
            'name': 'Cancelled'
          }]
        },
        searchDef: {
          props: ['flow_no', 'state_code']
        },
        actionColDef: {
          minWidth: '200',
          def: [{
            handler: row => {
              this.$message('Edit clicked')
              console.log('Edit in row clicked', row)
            },
            name: 'Edit'
          }, {
            icon: 'message',
            type: 'text',
            handler: row => {
              this.$message('RUA in row clicked')
              console.log('RUA in row clicked', row)
            },
            name: 'RUA'
          }]
        },
        paginationDef: {
          layout: 'prev, pager, next, jumper, sizes, total',
          pageSize: 1,
          pageSizes: [1, 2, 3]
        }
      }
    },
    created() {
      console.log('created')
    },
    methods: {
      change(val) {
      },
      handleSelectChange(selection) {
        this.selection = selection
        console.log(selection)
      },
      handleRowClick() {
        console.log('clicked')
      },
      sortM(sortData) {
        if (sortData.order) {
          let order = sortData.order
          let prop = sortData.prop
          let isDescending = order === 'descending'

          this.tableData.sort(function (a, b) {
            let flag = a[prop] - b[prop]
            if (Number.isNaN(flag)) return a[prop] && b[prop] && (a[prop] + '').localeCompare(b[prop], 'cn')
            return flag
          })
          if (isDescending) {
            this.tableData.reverse()
          }
        }
      }
    }
  }
</script>
