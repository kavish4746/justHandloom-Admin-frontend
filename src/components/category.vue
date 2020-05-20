<template>
    <div @click="onclickmaindiv">
        <sidebar></sidebar>
        <div class="maindiv">

            <!-- <div class="card" v-if="flag"> 
                    <form>
                        <input type="text" placeholder="Enter Category Name" v-model="catarr[0].cat_name"><br>
                        <button @click="addcat" class="button2 btnPush btnBlueGreen">Add Category</button>
                    </form>
            </div> -->
    <div><button @click="onclickadd" class="button1">Add new category</button></div>
    <md-table class="md-table" v-model="searched" md-sort="name" md-sort-order="asc" md-card md-fixed-header>
        <md-table-toolbar>
            <div class="md-toolbar-section-start">
                <h1 class="md-title">Category List</h1>
            </div>
            <md-field md-clearable class="md-toolbar-section-end">
                <br><md-input placeholder="Search by name..." v-model="search" @input="searchOnTable" />
            </md-field>
      </md-table-toolbar>


        <md-table-row slot="md-table-row" slot-scope="{ item }">
            <md-table-cell  md-label="Category ID" md-sort-by="cat_id" md-numeric>{{ item.cat_id }}</md-table-cell>
            <md-table-cell  md-label="Category Name" md-sort-by="cat_name" md-numeric>{{ item.cat_name }}</md-table-cell>
            <md-table-cell md-label="Delete Category" md-numeric>
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
      return items.filter(item => toLower(item.cat_name).includes(toLower(term)))
    }

    return items
  }




import categoryclass from '../services/category'
export default {
    data:function(){
        return{
            userarr:[],
            categoryarr:[{}],
            search: null,
            searched: [],
            catarr:[{
                cat_id:'',
                cat_name:''
            }],
            cnt:0,
            flag:false,
            flag1:true,
            currentPage: 1,
            total: 198,
            loading: false,
            paginationInfo: {
            text: '',

            },
        }
    },
    created(){
        categoryclass.getallcategory().then(doc=>{
            console.log(doc);
            this.cnt=doc.data[doc.data.length-1].cat_id;
            this.categoryarr=doc.data;
            this.searched = this.categoryarr;
            
            console.log(this.cnt);
        })
    },
    methods: {
        searchOnTable () {
        this.searched = searchByName(this.categoryarr, this.search)
        },
        onclickmaindiv:function(){
            this.flag1=true;
        },
        onclickadd:function(){
            this.flag=!this.flag;
            this.flag1=!this.flag1;

            this.$prompt("Please Enter Category Name").then((text1) => {
            this.catarr[0].cat_name=text1;
            this.catarr[0].cat_id=this.cnt+1;
            console.log(this.catarr[0]);
            categoryclass.addcat(this.catarr[0]).then(doc=>{
                console.log(doc);
                this.$fire({
					title: "Category successfully added",
					type: "success",
					timer: 3000
					}).then(r => {
						console.log(r.value);
					});
            })
            console.log(this.forgetemail);
            });
        },
        addcat:function(){
            this.flag=!this.flag;
            this.flag1=!this.flag1;    
        },
        onclickdelete:function(item){
            categoryclass.deletecat(item.cat_id).then(doc=>{
                for(let i=0;i<this.categoryarr.length;i++){
                if(this.categoryarr[i].cat_id == item.cat_id )
                {
                    this.categoryarr.splice(i,1);
                }
            }

            this.$fire({
					title: "category successfully Deleted",
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
tr:hover{
    background-color: #bce7e5;
}
.md-field {
    max-width: 350px;
  }
.md-table
{
    width:60%;
    margin-left:20%;
    background-color: white;
}
.md-title{
    font-family: 'Times New Roman', Times, serif;
    font-size:30px;
    color:black;
    left:0px;
}
.md-label
{
    text-align: center;
}
.md-input{
    border:1px black;
    border-bottom:1px solid black;
}
.maindiv{
    margin-left: 10%;
}
.button1{
  display: block;
  position: relative;
  width: 21%;
  padding: 0;
  margin: 10px 20px 10px 0;
  font-weight: 600;
  text-align: center;
  line-height: 50px;
  background-color: #8fd8d4;
  border-radius: 5px;
  border:none;
  margin-top:2%;
  font-size:22px;
  margin-left:20%;
  font-family: 'Times New Roman', Times, serif;
}
.button1:hover{
    background-color: #198e88;
}

td{
    font-size:20px;
}

</style>