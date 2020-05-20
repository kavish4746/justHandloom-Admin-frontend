<template>
 <div>
        <sidebar></sidebar>
        <div class="maindiv">
            
        <md-table class="md-table" v-model="searched" md-sort="name" md-sort-order="asc" md-card>
                                
                                <md-table-toolbar>
                                <router-link to="/order"><i style="font-size:30px;margin-top:-4%;margin-left:-3%;" class="fa fa-arrow-alt-circle-left"></i></router-link>
                                  <div class="md-toolbar-section-start">
                                  </div>  
                                  <div class="md-toolbar-section-start">
                                      
                                      <h3>Order_id:- {{o_id}}</h3>
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



import suborderclass from '../services/suborder'
import productclass from '../services/product'
export default {
  data:function(){
    return{
      o_id:'',
      suborderarr:[{}],
      search: null,
      searched: [{}],
    }
  },
  created(){
    this.o_id=this.$route.params.o_id;
    console.log(this.o_id);
    suborderclass.getallsuborders(this.o_id).then(doc=>{
        this.searched=doc.data;
    })
    
    console.log(this.suborderarr);
    console.log(this.searched);
  },
  methods:{
        searchOnTable () {
        this.searched = searchByName(this.suborderarr, this.search)
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
