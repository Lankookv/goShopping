<template>
  <div class="modal-backdrop">
    <div style="overflow:hidden;width:50%;">
    <div class="modal" >
      <div class="modal-header">
        <h2 style="margin-top: 0;margin-bottom: 0">冻结商品信息</h2>
        <img src="../../components/icon/关闭.png" class="iconfont" @click="closeSelf" style="float: right;margin-right: 3%;cursor: pointer;">
      </div>
      <div class="modal-body">
        <el-table
          :row-class-name="tableRowClassName"
          :data="frozenGoodsBuyerList"
          max-height=80%
          style="width: 100%"size="20px">
          <el-table-column
            prop="buyerName"
            label="姓名"
            width=120px>
          </el-table-column>
<!--          <el-table-column-->
<!--            prop="number"-->
<!--            label="数量"-->
<!--            width=100px>-->
<!--          </el-table-column>-->
          <el-table-column
            prop="date"
            label="时间"
            width=200px>
          </el-table-column>
          <el-table-column
            fixed="right"
            label="操作"
            width=200px>
            <template slot-scope="scope">
              <el-button  @click="putOnGood(scope.row.dealId)" id="putOnGood">上架</el-button>
<!--              <putOnGoodModal v-on:closeme1="closeme1" id="putOnGoodModal"></putOnGoodModal>-->
              <el-button  @click="putOffGood(scope.row.dealId)" id="putOffGood">下架</el-button>
<!--              <putOffGoodModal v-show="showModal2" v-on:closeme2="closeme2" :buyerId="scope.row.id"></putOffGoodModal>-->
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
  import {putOnGood, putOffGood,showFrozenGoodBuyerInformation} from '../../api';
  import putOffGoodModal from "../../components/putOffGoodModal"
  import putOnGoodModal from "../../components/putOnGoodModal"

export default {
  name: 'frozenGoodBuyerInformationModal',
  components:{
    putOffGoodModal,
    putOnGoodModal
  },
  props: {
    // data:{
    //   type: [Object, Array],
    //   default: []
    // },
    goodId:{
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      showModal1:false,
      showModal2:false,
      // frozenGoodsBuyerList:[{id:1,name:"a",number:2,time:"2021.10.10 20:25"},{id:2,name:"b",number:2,time:"20:25"},{id:3,name:"c",number:2,time:"2021.10.10 20:25"},{id:4,name:"d",number:2,time:"20:25"},{id:5,name:"e",number:2,time:"2021.10.10 20:25"},{id:6,name:"f",number:2,time:"20:25"},{id:1,name:"a",number:2,time:"2021.10.10 20:25"},{id:2,name:"b",number:2,time:"20:25"},{id:3,name:"c",number:2,time:"2021.10.10 20:25"},{id:4,name:"d",number:2,time:"20:25"},{id:5,name:"e",number:2,time:"2021.10.10 20:25"},{id:6,name:"f",number:2,time:"20:25"},],
      frozenGoodsBuyerList:[],
    }
  },
  created() {
    this.getFrozenGoodBuyerInformation();
  },
  methods: {
    getFrozenGoodBuyerInformation(){
      showFrozenGoodBuyerInformation({
        goodId:JSON.stringify(this.$route.params.bid),
        contentType: "application/json"
      })
        .then((response)=> {
          // alert("JSON.stringify(this.$route.params.bid):"+this.$route.params.bid)
          this.frozenGoodsBuyerList=response.data.data.dealList;
        })
    },
    closeSelf() {
      this.$emit("closeme");
    },
    tableRowClassName({row, rowIndex}) {
      row.index = rowIndex;
    },
    putOnGood(id){
      // this.showModal1=!this.showModal1;
      // alert(this.showModal1);
      putOnGood({
        dealId:id,
        contentType: "application/json"
      })
        .then((response)=> {
          if(response.data.code===-1){
            this.$message.error('上架失败！');
            this.closeSelf();
          }
          else{
            this.$message.success('上架成功！');
            this.closeSelf();
          }
        })
    },
    putOffGood(id){
      // this.showModal2=!this.showModal2;
      // alert(this.showModal2);
      putOffGood({
        dealId:id,
        contentType: "application/json"
      })
        .then((response)=> {
          if(response.data.code===-1){
            this.$message.error('下架失败！');
            this.closeSelf();
          }
          else{
            this.$message.success('下架成功！');
            this.closeSelf();
          }
        })
    },
    closeme1(){
      alert(this.showModal1)
      this.showModal1=!this.showModal1;
      alert(this.showModal1)
    },
    closeme2(){
      this.showModal2=!this.showModal2;
    },
  },
}
</script>

<style scoped lang="less">
.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0,0,0,.3);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}
.modal {
  background-color: #fff;
  box-shadow: 2px 2px 20px 1px;
  overflow-y: scroll;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  width: 105%;
  height:auto;
  min-height:300px;
  max-height:400px;
}
.modal-header {
  border-bottom: 1px solid #eee;
  color: #313131;
  justify-content: space-between;
  padding: 8px;
  display: flex;
}
.modal-footer {
  border-top: 1px solid #eee;
  justify-content: flex-end;
  padding: 15px;
  display: flex;
}
.modal-body {
  position: relative;
  padding: 10px 10px;
  input{
    font-size: 15px;
  }
}
.btn-close, .btn-confirm {
  border-radius: 8px;
  margin-left:16px;
  width:56px;
  height: 36px;
  border:none;
  cursor: pointer;
}
.btn-close {
  color: #313131;
  background-color:transparent;
}
.btn-confirm {
  color: #fff;
  background-color: #2d8cf0;
}
.el-table .el-table__cell{
  padding: 6px 0;
}
.el-table .cell{
  line-height: 0px;
}
.el-button {
  padding: 5px 20px;
}
.iconfont{
  font-family:"iconfont" !important;
  width:16px;
  height: 16px;
  font-style:normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}

</style>
