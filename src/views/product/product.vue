<template>
  <div>
    <div class="title-box">
      商品查询
    </div>
    <div class="section-box">
      <v-search />
      <v-result :getSearchList="getProductList">
        <template slot="slot-detail" slot-scope="scope" >
          <p>slot数据传回：{{scope.rs.brand}}</p>
          <p>接口方法参数传递：{{scope.rs.price}}</p>
        </template>
        <template slot="slot-operate" slot-scope="scope" >
          <el-button size="small" type="primary" @click="editDetail(scope.rs)">编辑</el-button>
        </template>

      </v-result>
    </div>
  </div>
</template>
<script>
  import {mapActions} from 'vuex'
  import pageMix from '@/components/Mixin/searchPage'
  import {Search,Result} from 'vue-bg-search'
  export default{
    mixins:[pageMix],
    created(){
      this.initView()
    },
    data:()=>({
      searchPage:{
        searchItem:[
          {
            type:'',
            key:'page',
            intKey:1
          },
          {
            type:'',
            key:'rows',
            intKey:20
          },
          {
            type:'input',
            label:'商品ID',
            placeholder:'',
            key:'baseGoodsCode',
            intKey:'', //初始值
            isClear:true, //重置是否清空
            regType:[
              {
                type:'Num',
                msg:'ID请输入数字'
              }
            ],
          },
          {
            type:'input',
            label:'商品名称',
            placeholder:'',
            key:'baseGoodsName',
            intKey:'', //初始值
            isClear:true, //重置是否清空
          },
          {
            type:'select',
            label:'出售状态',
            filterable:false, //是否可模糊匹配
            placeholder:'',
            key:'status',
            intKey:'',
            selectType:'selectAll',
            options:[
              {
                value: 'all',
                label: '全部'
              },
              {
                label:'出售中',
                value:'BB'
              },
              {
                label:'待出售',
                value:'CC'
              },
            ]
          },
          {
            type:'select',
            label:'商品分组',
            filterable:true, //是否可模糊匹配
            placeholder:'',
            key:'managerGroupId',
            intKey:'',
            options:[
              {
                label:'A',
                value:'BB'
              },
            ]
          },
        ],
        searchMenu:[
          {
            label:'查询',
            type:'search',
            style:'primary',
          },
          {
            isSlot:true,
            slotName:'private-menu',
          }
        ],
        searchResult:[
          {
            label:'ID',
            key:'id',
            width:'120',
            align:'center'
          },
          {
            label:'主图',
            key:'imageUrl',
            width:'120',
            type:'image',
            align:'center'
          },
          {
            label:'名称',
            key:'name',
            align:'center'
          },
          {
            label:'详情',
            type:'slot',
            width:'580',
            slotName:'slot-detail',
            key:'detail',
            align:'center'
          },
          {
            label:'操作',
            key:'operate',
            width:'160',
            type:'slot-operate',
            slotName:'slot-operate',
            align:'center',
            items:[
              {
                label:'编辑',
                type:'primary'
              },
              {
                label:'下架'
              }
            ]
          }
        ],
      },

    }),
    methods:{
      ...mapActions({
        getProductList: 'product/getProductList'
      }),
      initView(){
        this.paramsInit()
      },
      editDetail(rs){
        console.log(rs)
      },

    },
    components:{
      vResult:Result,
      vSearch:Search
    },
  }
</script>
