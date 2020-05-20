<template>
 <div>
        <sidebar></sidebar>
        <div class="maindiv">
            
        <md-table class="md-table" v-model="searched" md-sort="name" md-sort-order="asc" md-card>
                                
                                <md-table-toolbar>
                                <router-link to="/bill"><i style="font-size:30px;margin-top:-4%;margin-left:-3%;" class="fa fa-arrow-alt-circle-left"></i></router-link>
                                  <div class="md-toolbar-section-start">
                                  </div>  
                                  <div class="md-toolbar-section-start">
                                      
                                      <h3>Bill_id:- {{bill_id}}</h3>
                                  </div>

                                  <md-field md-clearable class="md-toolbar-section-end">
                                      
                                  </md-field>
                                  </md-table-toolbar>


                <md-table-row slot="md-table-row" slot-scope="{ item }">
                        <md-table-cell md-label="Product Name" md-sort-by="fk_p_name" md-numeric >{{ item.fk_p_name}}</md-table-cell>
                        <md-table-cell md-label="Ordered quantity" md-sort-by="fk_p_qty" >{{ item.fk_p_qty}}</md-table-cell>
                        <md-table-cell md-label="Product price" md-sort-by="fk_p_price"><span style="color:lightblue">Rs. {{ item.fk_p_price }}</span></md-table-cell>
                        <md-table-cell md-label="Total Price" md-sort-by="o_price"><span style="color:blue">Rs. {{ item.c_price}}</span></md-table-cell>
                </md-table-row>
        </md-table>
        <md-table class="md-table" v-model="searched" md-sort="name" md-sort-order="asc" md-card>
                <md-table-row slot="md-table-row">
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:Black;font-size:20px;"><b> Total amount(without tax)</b></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:black;font-size:20px;">Rs. {{total}}.00</span></md-table-cell>
                </md-table-row>
        </md-table>            
        <md-table class="md-table" v-model="searched" md-sort="name" md-sort-order="asc" md-card>
                <md-table-row slot="md-table-row">
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:Black;font-size:20px;"><b>+ SGST(9%) CGST(9%)</b></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:black;font-size:20px;">+ {{tax}}</span></md-table-cell>
                </md-table-row>
        </md-table>

        <md-table class="md-table" v-model="searched" md-sort="name" md-sort-order="asc" md-card>
                <md-table-row slot="md-table-row">
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:gray;font-size:22px;"><b>Total amount(incl. GST)</b></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:blue;"></span></md-table-cell>
                    <md-table-cell><span style="color:Red;font-size:22px;">Rs. {{totalp}}.00</span></md-table-cell>
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



import billdetailclass from '../services/billdetail'

export default {
  data:function(){
    return{
      bill_id:'',
      billdetailarr:[{}],
      search: null,
      searched: [{}],
      total:0,
      totalp:0,
      tax:0,
      sgst:0,
      cgst:0,
    }
  },
  created(){
    this.bill_id=this.$route.params.bill_id;
    console.log(this.bill_id);
    billdetailclass.getallbilldetail(this.bill_id).then(doc=>{
        this.searched=doc.data;
        this.billdetailarr=doc.data;

        for(let i=0;i<this.billdetailarr.length;i++)
        {
                
            this.total+=this.billdetailarr[i].c_price;
            this.sgst=this.total*0.09;
            this.cgst=this.total*0.09;
            
            
        }
        this.tax+=this.sgst;
        this.tax+=this.cgst;
        this.totalp=this.total+this.tax;
        this.totalp=this.totalp.toFixed(0);
    })
    
  },
  methods:{
        searchOnTable () {
        this.searched = searchByName(this.billdetailarr, this.search)
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
  }
button{
    height:30px;
    border:none;
    background-color:lightsalmon;
}
.md-table{
    width:72%;
    margin-left:12%;
}
</style>
