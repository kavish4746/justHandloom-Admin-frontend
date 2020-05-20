<template>
 <div>
        <sidebar></sidebar>
        <div class="maindiv">      
        <md-table v-model="searched" md-sort="name" md-sort-order="asc" md-card>

                                  <md-table-toolbar>
                                  <div class="md-toolbar-section-start">
                                  <button @click="onclickall" class="btn btn-sm btn-outline-success"><b>All Orders</b></button>&nbsp;&nbsp;
                                  <button @click="onclickpending" class="btn btn-sm btn-outline-success"><b>Pending Orders</b></button>&nbsp;&nbsp;
                                  <button @click="onclickcompleted" class="btn btn-sm btn-outline-success"><b>Completed orders</b></button>&nbsp;&nbsp;
                                  </div>

                                  <md-field md-clearable class="md-toolbar-section-end">
                                      <md-input style="border:1px solid lightgrey;" placeholder=" Search by user email..." v-model="search" @input="searchOnTable" />
                                  </md-field>
                                  </md-table-toolbar>

                                    <md-table-row slot="md-table-row" slot-scope="{ item }">
                                    <md-table-cell md-label="User Email_Id" md-sort-by="fk_email_id" >{{ item.fk_emailid}}</md-table-cell>
                                    <md-table-cell md-label="Order Date" md-sort-by="o_date">{{ item.o_date | moment("Do MMMM YYYY")}}</md-table-cell>
                                    <md-table-cell md-label="Order amount" md-sort-by="o_price"><span style="color:blue">Rs. {{ item.o_price}}</span></md-table-cell>
                                    <md-table-cell md-label="Address" md-sort-by="fk_address">{{ item.fk_address}}</md-table-cell>
                                    <md-table-cell md-label="Status" v-if="item.status=='pending'"  md-sort-by="status"><button  @click="onclickstatus(item)"  style="border:none;background-color:white;"><span style="color:red;text-transform: uppercase;">{{ item.status}}</span></button></md-table-cell>
                                    <md-table-cell md-label="Status" v-if="item.status=='done'" style="text-transform: uppercase;" md-sort-by="status"><span style="color:green;">{{ item.status}}</span></md-table-cell>
                                    <md-table-cell md-label="Option">
                                    <router-link v-bind:to="'/orderdetail/' + item.o_id"  class="btn btn-sm btn-outline-success">Order detail</router-link>
                                    </md-table-cell>

                                    </md-table-row>
                                    </md-table>
        </div>
    </div>
</template>

<script>

  const toLower = text => {
    return text.toString().toLowerCase()
  }

  const searchByName = (items, term) => {
    if (term) {
      return items.filter(item => toLower(item.fk_emailid).includes(toLower(term)))
    }

    return items
  }



import orderclass from '../services/order'
import suborderclass from '../services/suborder'
import productclass from '../services/product'
import billclass from '../services/bill'
import billdetailclass from '../services/billdetail'
export default {
  data:function(){
    return{
      o_id:'',
      orderarr:[{}],
      search: null,
      searched: [],
      billarr:[{
         bill_id:'' ,
         o_price:'',
         o_date:'',
         fk_emailid:'',
         fk_address:'',
      }],
      billdetailarr:[{}],
    }
  },
  created(){
    orderclass.getallorders().then(doc=>{
        this.orderarr=doc.data;
        this.searched=doc.data;
    })
  },
  methods:{
        searchOnTable () {
        this.searched = searchByName(this.orderarr, this.search)
    },
    onclickpending:function(){
        orderclass.getorderbystatus("pending").then(doc=>{
            this.orderarr=doc.data;
            this.searched=doc.data;
        })
    },
    onclickcompleted:function(){
        orderclass.getorderbystatus("done").then(doc=>{
            this.orderarr=doc.data;
            this.searched=doc.data;
        })
    },
    onclickall:function(){
      orderclass.getallorders().then(doc=>{
        this.orderarr=doc.data;
        this.searched=doc.data;
    })
    },
    onclickstatus:function(item){
        console.log(item) ;
        item.status="done";
        console.log(item) ;
        orderclass.updateorderstatus(item).then(doc=>{
            console.log(doc);
            this.billarr[0].bill_id=Math.random()*100;
            this.billarr[0].o_price=item.o_price;
            this.billarr[0].o_date=item.o_date;
            this.billarr[0].fk_emailid=item.fk_emailid;
            this.billarr[0].fk_address=item.fk_address;
            console.log(this.billarr[0]);
            billclass.addbill(this.billarr[0]).then(billdoc=>{
                console.log(billdoc);
            
                    suborderclass.getallsuborders(item.o_id).then(suborderdoc=>{
                        for(let i=0;i<suborderdoc.data.length;i++)
                        {

                            productclass.getproductbyid(suborderdoc.data[i].fk_p_id).then(prod=>{
                                    console.log("Product data" + prod);
                                   prod.data[0].p_qty-=suborderdoc.data[i].fk_p_qty;
                                   productclass.updateproduct(prod.data[0]).then(produpdate=>{
                                        console.log("Product update data" + produpdate.data);
                                   })//produpdate
                            })//prod
                        
                            console.log(suborderdoc.data[i]);
                            this.billdetailarr[i]={
                                fk_bill_id:this.billarr[0].bill_id,
                                fk_p_id:suborderdoc.data[i].fk_p_id,
                                fk_p_name:suborderdoc.data[i].fk_p_name,
                                fk_p_qty:suborderdoc.data[i].fk_p_qty,
                                fk_p_price:suborderdoc.data[i].fk_p_price,
                                c_price:suborderdoc.data[i].c_price
                            }
                            console.log(this.billdetailarr[i]);
                            billdetailclass.addbilldetail(this.billdetailarr[i]).then(billdetaildoc=>{
                                console.log(billdetaildoc);
                            })//billdetail

                        }//loop
                    })//suborder
            })//billadd
        })

    }  
  }
}
</script>

<style scoped>
.maindiv{
    margin-left: 10%;
}
.md-field {
    max-width: 350px;
  }

  tr:hover{
      background-color: f1f1f1;
      
  }
  h2,td{
      font-size:16px;
      font-family:'Times New Roman', Times, serif;
      font-style: fa-bold;
  }
button{
    height:30px;
    border:none;
    background-color:lightsalmon;
}
</style>
