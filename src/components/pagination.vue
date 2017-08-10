<template>
  <div class="pagination">
    <div class="left">
      <div class="detail-info">
        <span>{{start}}-{{end}}条</span>
        <span>共{{total}}条</span>
      </div>
      <el-select v-model="pageSizeInner" @change="pageSizeChange">
        <el-option v-for="item in pageSizesObj"
                   :key="item.value"
                   :label="item.label"
                   :value="item.value"></el-option>
      </el-select>
    </div>
    <div class="right">
      <div class="page-index">
        <span v-if="total">{{currentPageInner}}</span>
        <span v-else>0</span>
        <span>/{{totalPage}}</span>
      </div>
      <div class="page-btn">
        <el-button :disabled="currentPageInner === 1 || total === 0" @click="currentChange('goFirst')"><i
          class="iconfont icon-page_first"></i></el-button>
        <el-button :disabled="currentPageInner === 1 || total === 0" @click="currentChange('pagePre')">
          <i class="iconfont icon-arrow-left"></i>上一页
        </el-button>
        <el-button :disabled="currentPageInner === totalPage || total === 0" @click="currentChange('pageNext')">
          下一页<i class="iconfont icon-arrow-right"></i>
        </el-button>
        <el-button :disabled="currentPageInner === totalPage || total === 0" @click="currentChange('goEnd')"><i
          class="iconfont icon-page_last"></i></el-button>
      </div>
      <div class="jumper">
        <span>跳至</span>
        <el-input-number type="number" v-model="jumpIndex"
                         :min="1"
                         :max="totalPage"
                         @keyup.enter.native="currentChange('jumpTo')">
        </el-input-number>
        <el-button type="success" :disabled="currentPageInner === jumpIndex || total === 0"
                   @click="currentChange('jumpTo')">跳转
        </el-button>
      </div>
    </div>
  </div>
</template>
<style lang="scss">
  @import './pagination.scss';
</style>
<script>
  export default {
    props: {
      total: {
        type: Number,
        default: 0
      },
      pageSizes: {
        type: Array,
        default: () => [10, 20, 30]
      },
      pageSize: {
        type: Number,
        default: 10
      },
      currentPage: {
        type: Number,
        default: 1
      }
    },
    data() {
      return {
        jumpIndex: 1,
        currentPageInner: this.currentPage,
        pageSizeInner: this.pageSize
      }
    },
    watch: {
      currentPageInner(val) {
        this.jumpIndex = val
      }
    },
    computed: {
      pageSizesObj() {
        return this.pageSizes.map((item) => {
          return {
            value: item,
            label: item + '条/页'
          }
        })
      },
      start() {
        let temp = (this.currentPageInner - 1) * this.pageSizeInner + 1
        return this.total ? temp : 0
      },
      end() {
        let temp = this.currentPageInner * this.pageSizeInner
        return temp > this.total ? this.total : temp
      },
      totalPage() {
        let temp = Math.ceil(this.total / this.pageSizeInner)
        return temp
      }
    },
    methods: {
      pageSizeChange() {
        if (this.currentPageInner > this.totalPage) {
          this.currentPageInner = this.totalPage
        }
        this.$emit('size-change', this.pageSizeInner)
      },
      jumpIndexChange(val) {
        this.jumpIndex = val
        this.currentChange('jumpTo')
      },
      currentChange(type) {
        switch (type) {
          case 'goFirst':
            this.currentPageInner = 1
            break
          case 'goEnd':
            this.currentPageInner = this.totalPage
            break
          case 'pageNext':
            if (this.currentPageInner < this.totalPage) {
              this.currentPageInner++
            }
            break
          case 'pagePre':
            if (this.currentPageInner > 1) {
              this.currentPageInner--
            }
            break
          case 'jumpTo':
            this.currentPageInner = this.jumpIndex
            break
          default :
            this.currentPageInner = 1
        }
        this.$emit('current-change', this.currentPageInner)
      }
    }
  }
</script>
