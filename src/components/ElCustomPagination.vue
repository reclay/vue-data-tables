<template>
  <div class="el-custom-pagination">
    <ul>
      <li v-for="pageIndex in pageCount"
          :class="[pageIndex===currentPage?'active':'']"
          @click="currentChange(pageIndex)"></li>
    </ul>
  </div>
</template>

<style lang="scss" rel="stylesheet/scss">
  $li-width: 15px;
  .el-custom-pagination{
    list-style: none;
    ul{
      padding: 0;
    }
    li{
      display: inline-block;
      width: $li-width;
      height: $li-width;
      background-color: #97a8be;
      border-radius: $li-width / 2;
      cursor: pointer;
      margin-left: 10px;
      &.active{
        background-color: #20a0ff;
      }
    }
  }
</style>

<script>
  export default {
    props: {
      currentPage: {
        type: Number
      },
      pageSize: {
        type: Number
      },
      total: {
        type: Number
      }
    },
    computed: {
      pageCount() {
        return Math.ceil(this.total / this.pageSize)
      }
    },
    methods: {
      currentChange(currentPage) {
        this.$emit('current-change', currentPage)
      }
    }
  }
</script>
