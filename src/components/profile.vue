<template>
    <div>
        <sidebar></sidebar>
        <div class="maindiv">
                 <div class="card">
                    <img src="../images/profilekavish.jpg" alt="John">
                    <br>
                    <h1>{{infoarr[0].u_name}}</h1>
                    <p class="title">{{infoarr[0].u_type}}</p>
                    <i class="fa fa-envelope"></i><p class="title1">{{infoarr[0].u_email}}</p>
                    <i class="fa fa-map-marker-alt"></i><p class="title">{{infoarr[0].u_address}}</p>
                    <p class="title">{{infoarr[0].u_city}}</p>
                    <p class="title">{{infoarr[0].u_state}}</p>
                    <i class="fas fas-phone-alt"></i><p class="title" style="color:blue;">{{infoarr[0].u_mno}}</p>
                    <p><router-link to="/editprofile"><button>Update Profile</button></router-link</p>
                    <p><button @click="updatepassword">Update Password</button></p>
                </div>
        </div>
    </div>
</template>

<script>
import infoclass from '../services/profile'
export default {
  name: 'app',
  data () {
    return {
      infoarr:[],
      oldpassword:'',
      newpassword:'',
    }
  },
  methods:{
      updatepassword:function(){
          this.$prompt("Please your Old password").then((text1) => {
            this.oldpassword=text1;
            console.log(this.oldpassword);
            console.log(this.infoarr[0].u_password);
            if(this.infoarr[0].u_password==this.oldpassword)
            {
                    this.$prompt("Enter your new password").then((newpass)=>{
                     this.infoarr[0].u_password=newpass;
                     infoclass.updateinfodetails(this.infoarr).then(doc=>{
                        console.log(doc);
                                this.$fire({
                                title: "profile successfully updated",
                                type: "success",
                                timer: 3000
					            }).then(r => {
						        console.log(r.value);
                            });   
                    
                            this.$router.push({path:"/profile"})
                        })
                    })          
                    
            }
            else
            {
                    this.$fire({
					title: "Password Doesn't Match",
					type: "warning",
					timer: 3000
					}).then(r => {
						console.log(r.value);
                    });
            }
        });
      }
  },
  created(){
    infoclass.getEmpdetails(localStorage.getItem('emailid')).then(doc=>{
      this.infoarr=doc.data;
      console.log(this.infoarr);
      console.log(localStorage.getItem('emailid'));
    })
  }
}
</script>

<style scoped>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  max-width: 45%;
  margin: auto;
  text-align: center;
  font-family: arial;
}

.title {
  color: grey;
  font-size: 18px;
  text-transform: uppercase;
}


.title1 {
  color: grey;
  font-size: 18px;
}

button {
  border: none;
  outline: 0;
  display: inline-block;
  padding: 8px;
  color: white;
  background-color: #000;
  text-align: center;
  cursor: pointer;
  width: 100%;
  font-size: 18px;
}

a {
  text-decoration: none;
  font-size: 22px;
  color: black;
}
img
{
    border-radius:500px;
    height:50%;
    width:50%;
}
button:hover, a:hover {
  opacity: 0.7;
}
</style>
