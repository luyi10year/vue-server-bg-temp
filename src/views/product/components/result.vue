<template>
  <div class="item-box">
    <div class="item-table">
      <div class="item-table">
        <el-table v-loading="loading" element-loading-text="拼命加载中" :data="searchResult.rows" style="width: 100%;" >
          <template v-for="(item,index) in searchConfig.searchResult">
            <el-table-column v-if="item.type=='image'" :label="item.label" :width="item.width" :align="item.align">
              <template slot-scope="scope">
                <div class="table-image">
                  <img :src="scope.row[item.key]" style="width: 80px;height: 80px" alt="">
                </div>
              </template>
            </el-table-column>
            <el-table-column v-else-if="item.type=='slot'" :label="item.label" :width="item.width" :align="item.align">
              <template slot-scope="scope">
                <slot :name="item.slotName" :rs="scope.row[item.key]"></slot>
                <!--<div class="table-detail">-->
                  <!--{{scope.row[item.key].brand}}-->
                <!--</div>-->
              </template>
            </el-table-column>
            <el-table-column v-else-if="item.type=='operate'" :label="item.label" :width="item.width" :align="item.align">
              <template slot-scope="scope">
                <template v-for="operate in item.items">
                  <el-button size="small" :type="operate.type" >{{operate.label}}</el-button>
                </template>
              </template>
            </el-table-column>
            <el-table-column v-else :prop="item.key" :label="item.label" :width="item.width" :align="item.align"></el-table-column>
          </template>
        </el-table>
      </div>
      <v-pagination v-if="pagingShow" :paging="paging" />
    </div>
  </div>
</template>
<script>
  import pageMix from '@/components/Mixin/resultPage'
  import {mapActions,mapState} from 'vuex'

  export default{
    mixins:[pageMix],
    created(){
      this.initView()
      // this.$parent.AAAA('parent')
      // this.$emit('clickBBBB','emit')
    },
    data:()=>({

    }),
    methods:{
      // ...mapActions('product',['getProductList']),
      ...mapActions({
        getProductList: 'product/getProductList'
      }),
//      initView(){
//        const query = UtilTool.parseQuery(this.$route.query)
//        const params = UtilTool.paramsAssign(query,this.searchParams)
//        this.setSearchParams(params)
//        this.searchList(params)
//      },
//       searchList(params){
//         this.loading = true
//         this.getProductList(params)
//           .then((rs)=>{
//               this.searchResult = rs
//               this.setPage()
//               this.loading = false
//               this.demoAsync(params)
//           },()=>{
//              this.loading = false
//           })
//
//
//     },
      async searchList(params){
        this.loading = true
        try {
          const rs = await this.getProductList(params)
          this.searchResult = rs
          this.setPage()
          this.loading = false
        } catch(err){
          this.loading = false
        }
        // this.demoAsync(params)
      },
      demoAsync(params){
        this.demo(params).then((rs)=>{
          console.log(rs)
        },()=>{
          console.log('=====')
        })
      },
      async demo(params){
        var a = ()=> new Promise((resolve => {
          resolve([12,34])
        }))
        const rs = await a()
        return rs
      },
    }
  }
</script>


