<template>
 <div>
        <sidebar></sidebar>
        <div class="maindiv">      
                    <!-- <div class="md-toolbar-section-start">
                        <button @click="allproduct"  class="btn btn-sm btn-outline-success catbutton1">All Products</button>&nbsp;&nbsp;
                        <button @click="productbycat(item)"  v-for="item in catarr" v-bind:key="item.cat_id" class="btn btn-sm btn-outline-success catbutton1">{{item.cat_name}}</button>&nbsp;&nbsp;
                    </div> -->
                    <div class="md-toolbar-section-end">
                            <span class="cat"><b>Select Category &nbsp;</b></span>
                            <select @change="productbycat()" v-model="selected">
                                <option v-bind:value="item.cat_id" v-for="item in catarr" v-bind:key="item.cat_id">{{item.cat_name}}</option>
                            </select>
                    </div>
                    <md-table v-model="searched" md-sort="name" md-sort-order="asc" md-card>

                                  <md-table-toolbar>
                          
                                  <md-field md-clearable class="md-toolbar-section-start">
                                      <button @click="addproduct" class="addprobtn">Add Product </button>
                                  </md-field>

                                  <md-field md-clearable class="md-toolbar-section-end">
                                      <md-input style="border:1px solid lightgrey;" placeholder=" Search by name..." v-model="search" @input="searchOnTable" />
                                  </md-field>

                                  
                                        
                                   
     
                                  
                                  </md-table-toolbar>

                                    <md-table-row slot="md-table-row" slot-scope="{ item }">
                                        <md-table-cell md-label="Product Image" md-sort-by="p_img"><img style="height:150px;width:500px;border-radius:100px;" v-bind:src="'http://localhost:3000/images/products/' + item.p_img"></md-table-cell>
                                    <md-table-cell md-label="Product Name" md-sort-by="p_name" >{{ item.p_name}}</md-table-cell>
                                    <md-table-cell md-label="Product Description" md-sort-by="p_des">{{ item.p_des}}</md-table-cell>
                                    <md-table-cell md-label="Product price" md-sort-by="p_price">Rs.{{ item.p_price}}</md-table-cell>
                                                                        <md-table-cell md-label="Product Stock" md-sort-by="p_qty">{{ item.p_qty}}</md-table-cell>
                                    <md-table-cell md-label="Buffer Stock" md-sort-by="buffer_stcok"><span class="btn btn-sm btn-outline-warning"><strong>{{item.buffer_stock}}</strong></span></md-table-cell>

                                    <md-table-cell md-label="Option">
                                    <router-link v-bind:to="'/editproduct/' + item.p_id"  class="btn btn-sm btn-outline-success">Edit</router-link>
                                    </md-table-cell>

                                    <md-table-cell md-label="Module">
                                      <button type="button" style="background-color:#ffcccc;border:none;" @click="onclickdelete(item)" class="btn btn-sm btn-outline-success">Delete</button>
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
      return items.filter(item => toLower(item.p_name).includes(toLower(term)))
    }

    return items
  }



import productclass from '../services/product'
import categoryclass from '../services/category'
export default {
  data:function(){
    return{
      p_id:'',
      productarr:[{}],
      catarr:[{}],
      search: null,
      searched: [],
      selected:'',
    }
  },
  created(){
    productclass.getallproduct().then(doc=>{
        this.productarr=doc.data;

        console.log(this.productarr);
        this.searched=doc.data;
    })

    categoryclass.getallcategory().then(doc=>{
        this.catarr=doc.data;
        this.catarr.splice(0,0,{"id":"new","cat_id":"","cat_name":"All Products"});
        console.log(this.catarr);
    })
  },
  methods:{
        searchOnTable () {
        this.searched = searchByName(this.productarr, this.search)
    },
    addproduct:function() {
        this.$router.push({path:'/addproduct'});
    },
    productbycat:function()
    {
        console.log(this.selected);
        
        if(this.selected=="")
        {
                productclass.getallproduct().then(doc=>{
                this.productarr=doc.data;
                this.searched=doc.data;
            })
        }
        else
        {
            productclass.productbycat(this.selected).then(doc=>{
                console.log(doc.data);  
                this.productarr=doc.data;
                this.searched=doc.data;
            })
        }
    },
    onclickdelete:function(item)
    {
        productclass.deleteproduct(item.p_id).then(doc=>{
            for(let i=0;i<this.productarr.length;i++){
                if(this.productarr[i].p_id == item.p_id )
                {
                    this.productarr.splice(i,1);
                }
            }

            this.$fire({
					title: "product successfully Deleted",
					type: "success",
					timer: 3000
					}).then(r => {
						console.log(r.value);
					});
        })
    }
  }
}
</script>

<style scoped>
.maindiv{
    margin-left: 10%;
}
.addprobtn{
    height: 50px;
    width:50%;
    background-color:#bce7e5;
    font-size:20px;;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}
.md-field {
    max-width: 350px;
  }

  tr:hover{
      background-color: whitesmoke;
  }
  h2,th,td{
      font-size:16px;
      font-family:'Times New Roman', Times, serif;
      font-style: fa-bold;
  }
  
select
{
    height:30px;
    width:30%;
    font-size:20px;
    font-family:'Times New Roman', Times, serif;
}
.cat
{
    font-size:20px;
    font-family:'Times New Roman', Times, serif;
}
</style>
