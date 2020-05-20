<template>
    <div>
        <sidebar></sidebar>
        <div class="maindiv">
                    <hr>
                    <br>
                    <div class="row clearfix">
                            <div class="col-md-6">
                                <div class="card">
                                <md-table v-model="searched" md-sort="name" md-sort-order="asc" md-card>

                                  <md-table-toolbar>      
                                        <div class="md-toolbar-section-start">
                                            <h1 class="md-title">Please fill below items</h1>
                                        </div>
                                  </md-table-toolbar>

                                  <md-table-row slot="md-table-row" slot-scope="{ item }">
                                        <md-table-cell md-label="Product Id" md-sort-by="p_id">{{ item.p_id}}</md-table-cell>
                                        <md-table-cell md-label="Product Name" md-sort-by="p_name" >{{ item.p_name}}</md-table-cell>
                                        <md-table-cell md-label="Product Quantity" md-sort-by="p_qty">{{ item.p_qty}}</md-table-cell>
                                        <md-table-cell md-label="Product price" md-sort-by="p_qty">Rs.{{ item.p_qty}}</md-table-cell>    
                                        <md-table-cell md-label="Add item Qty.">
                                        <button style="color:black;background-color:orange;border:none;">Fill item</button>    
                                        </md-table-cell>    
                                    </md-table-row>
                                    </md-table>

                                </div>
                            </div>

                            <div class="col-md-6">
                                <div class="card">
                                        <md-table v-model="searched1" md-sort="name" md-sort-order="asc" md-card>

                                  <md-table-toolbar>      
                                        <div v-if="slen1>=1" class="md-toolbar-section-start">
                                            <h1  class="md-title">Pending Order List</h1>
                                        </div>
        
                                        <div v-if="slen1==0" class="md-toolbar-section-start">
                                            <h1  class="md-title" style="color:lightseagreen;">Pending orders List</h1>
                                        </div>
                                  </md-table-toolbar>

                                  <md-table-row slot="md-table-row" slot-scope="{ item }">
                                        <md-table-cell md-label="User Email" md-sort-by="fk_emailid">{{ item.fk_emailid}}</md-table-cell>
                                        <md-table-cell md-label="Date of order" md-sort-by="o_date" >{{ item.o_date}}</md-table-cell>
                                        <md-table-cell md-label="Amount" md-sort-by="o_price">Rs.{{ item.o_price}}</md-table-cell>    
                                    </md-table-row>
                                    </md-table>

                                </div>
                            </div>

                    </div>
            <hr>
            <br>

                        <div class="row clearfix">
                            <div class="col-md-12">
                                <div class="card">
                                <md-table v-model="searched2" md-sort="name" md-sort-order="asc" md-card>

                                  <md-table-toolbar>      
                                        <div v-if="slen2>=1" class="md-toolbar-section-start">
                                            <h1 class="md-title">Reply to customer Messages</h1>
                                        </div>

                                        <div v-if="slen2==0" class="md-toolbar-section-start">
                                            <h1 class="md-title" style="color:lightseagreen;">Queries from customer</h1>
                                            
                                        </div>
                                  </md-table-toolbar>

                                  <md-table-row slot="md-table-row" slot-scope="{ item }">
                                        <md-table-cell md-label="User name" md-sort-by="user_name" md-numeric>{{ item.user_name}}</md-table-cell>
                                        <md-table-cell md-label="User EmailID" md-sort-by="user_emailid" md-numeric >{{ item.user_emailid}}</md-table-cell>
                                        <md-table-cell md-label="user Message" md-sort-by="user_message" md-numeric>{{ item.user_message}}</md-table-cell>
                                        <md-table-cell md-label="Direct Mail" md-sort-by="" md-numeric><span style="color:blue;cursor:pointer">Email</span></md-table-cell>
                                        
                                    </md-table-row>
                                    </md-table>
                                </div>
                            </div>
                        </div>
                        <br><br>    
                        <span class="title"><b>User Information</b></span>
                            <v2-table 
                                    :data="userarr" border 
                                    :loading="loading" 
                                    :total="total"
                                    :shown-pagination="true"
                                    :pagination-info="paginationInfo"
                                    @page-change="handlePageChange"
                                    :default-sort='{prop: "u_name", order: "descending"}' @sort-change="handleSortChange">
                                    <v2-table-column label="Name" prop="u_name"></v2-table-column>
                                    <v2-table-column label="Email" prop="u_email"></v2-table-column>
                                    <v2-table-column label="Address" prop="u_address" width="400"></v2-table-column>
                                    <v2-table-column label="city" prop="u_city"></v2-table-column>
                                    <v2-table-column label="state" prop="u_state"></v2-table-column>
                                    <v2-table-column label="mobile" prop="u_mob"></v2-table-column>
                            </v2-table>
                          
        </div>
    </div>
</template>


<script>
import dashboardclass from '../services/dashboard'
export default {
    data:function(){
        return{
            userarr:[],
            buffferstockarr:[{}],
            pendingorderarr:[{}],
            contactarr:[{}],
            searched: [],
            searched1: [],
            searched2: [],
            slen:'',
            slen1:'',
            slen2:'',
            currentPage: 1,
            total:0,
            loading: false,
            paginationInfo: {
            text: ''
            },
        }
    },
    created(){
        dashboardclass.getalluser().then(doc=>{
            console.log(doc);
            this.userarr=doc.data;
            this.total=this.userarr.length;
        })

        dashboardclass.getallcontactmsg().then(doc=>{
            console.log(doc);
            this.contactarr=doc.data;
            this.searched2=doc.data;
            this.slen2=this.searched2.length;
        })

        dashboardclass.getbufferstock().then(doc=>{
            console.log(doc);
            this.bufferstockarr=doc.data;
            this.searched=doc.data;
            this.slen=this.searched.length;
        })

        dashboardclass.getallpendingorder().then(doc=>{
            console.log(doc);
            this.pendingorderarr=doc.data;
            this.searched1=doc.data;
            this.slen1=this.searched1.length;
            console.log(this.slen1+"slen1");
        })
    },
    methods: {
      handlePageChange(page) {
        this.currentPage = page;
        this.loading = true;
        let start = this.userarr.length;

        const arr = this.userarr.map(item => {
          return Object.assign({}, item, {
            u_name: item.u_name,
          });
        });

        setTimeout(() => {
          this.loading = false;
          this.userarr = [].concat(arr);
        }, 2000);  
      },
      handleSortChange( {prop, order}) {
        // Customize your sorting method. Maybe it will get data from server.
        this.loading = true;
        let arr = [].concat(this.userarr);
        arr.sort((item1, item2) => {
          let val1 = '';
          let val2 = '';

          
          if (prop === 'u_name') {
            val1 = item1[prop];
            val2 = item2[prop]
            if (order === 'descending') {
              return val2 < val1 ? -1 : 1
            }
            return val1 < val2 ? -1 : 1
          }
        });
        setTimeout (() => {
          this.loading = false;
          this.userarr = [].concat(arr);
        }, 2000);
      }
    }
}
</script>


<style scoped>
.title{
    color:Black;
    font-size:25px;
    font-family:'Arial, Helvetica, sans-serif;';
}
td{
    color:red;
}
.maindiv{
    margin-left:8%;
}
</style>