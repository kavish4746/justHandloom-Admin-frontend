<template>
 <div>
        <sidebar></sidebar>
        <div class="maindiv">      
        <md-table v-model="searched" md-sort="name" md-sort-order="asc" md-card>

                                  <md-table-toolbar>
                                  <div class="md-toolbar-section-start">
                                      <h3>Bill Data</h3>
                                  </div>

                                  <md-field md-clearable class="md-toolbar-section-end">
                                      <md-input style="border:1px solid lightgrey;" placeholder=" Search by user email..." v-model="search" @input="searchOnTable" />
                                  </md-field>
                                  </md-table-toolbar>

                                    <md-table-row slot="md-table-row" slot-scope="{ item }">
                                    <md-table-cell md-label="User Email_Id" md-sort-by="fk_email_id" >{{ item.fk_emailid}}</md-table-cell>
                                    <md-table-cell md-label="Billing Date" md-sort-by="o_date">{{ item.o_date | moment("Do MMMM YYYY")}}</md-table-cell>
                                    <md-table-cell md-label="Bill amount" md-sort-by="o_price"><span style="color:blue">Rs. {{ item.o_price}}</span></md-table-cell>
                                    <md-table-cell md-label="Billing Address" md-sort-by="fk_address">{{ item.fk_address}}</md-table-cell>
                                    

                                    <md-table-cell md-label="Option">
                                    <router-link v-bind:to="'/billdetail/' + item.bill_id"  class="btn btn-sm btn-outline-success">Bill products detail</router-link>
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



import billclass from '../services/bill'
import billdetailclass from '../services/billdetail'

export default {
  data:function(){
    return{
      o_id:'',
      orderarr:[{}],
      search: null,
      searched: [],
      billarr:[{}],
    }
  },
  created(){
    billclass.getallbill().then(doc=>{
        this.billarr=doc.data;
        this.searched=doc.data;
    })
  },
  methods:{
        searchOnTable () {
        this.searched = searchByName(this.billarr, this.search)
    },
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
