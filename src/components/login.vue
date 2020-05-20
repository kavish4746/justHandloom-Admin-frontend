<template>
  <div class="maindiv">
    <div class="main">
  <h1><i>Just</i>Handloom</h1>
  <h6>one stop for shooping</h6>
  <div class="row">
    <div class="form-p">
      <form>
        <input class="text" type="email" v-model="email" placeholder="Username" name="Username" required>
        <input class="text" type="password" v-model="password" placeholder="Password" name="Password" required>
        <input type="button" v-on:click="onlogin" value="Login">
      </form>

      <p style="margin-top:2%;"><a @click="forgetpassword">Forget your password ?</a></p>


    </div>
  </div>
</div>
  </div>
</template>

<script>
import loginclass from '../services/login'
export default {
  
  data () {
    return {
      email:"",
      password:"",
      signuparr:[{}],
      forgetemail:'',
      mail:{
        to:'',
        subject:'',
        message:''
      }
    }
  },
  methods:{
      onlogin:function(){
          loginclass.getEmpByEmailPassword(this.email,this.password).then(doc=>{
              console.log(doc.data.length);
            if(doc.data.length>0)
            {
                if(doc.data[0].u_type=='admin')
                {
                        console.log(doc.data);
                        localStorage.setItem('emailid',this.email);
                        console.log(localStorage.getItem('emailid'))
                            this.$fire({
                            title: "Login successfully",
                            type: "success",
                            timer: 3000
                            }).then(r => {
                                console.log(r.value);
                            });   
                        this.$router.push({path:'/dashboard/' + this.email});
                }
                else
                {
                            this.$fire({
                            title: "You don't have access",
                            type: "warning",
                            timer: 3000
                            }).then(r => {
                                console.log(r.value);
                            });
                }
            }
            else
            {
                            this.$fire({
                            title: "Emailid and Password doesn't match ",
                            type: "warning",
                            timer: 3000
                            }).then(r => {
                                console.log(r.value);
                            });
            }
          })
      },
      forgetpassword:function(){
        this.$prompt("Please your Emailid").then((text1) => {
            this.forgetemail=text1;
            loginclass.getpassword(this.forgetemail).then(doc=>{
              console.log(doc.data[0].u_password);
              this.mail.to=this.forgetemail;
              this.mail.subject="Password Details Of your JustHandllom Account";
              this.mail.message="Your Password is :- "+doc.data[0].u_password;
              loginclass.sendmail(this.mail).then(doc=>{
                console.log(doc);
              })

            })
            console.log(this.forgetemail);
        });
      },
  }
}
</script>

<style scoped>

.maindiv {
 margin:0;
 padding:0;
  font-family: "trebuchet ms";
  background: #e8f7f6;
  height:100vh;
  margin-top:-4.5%;
}

h1 {
  font-family: 'Dosis', "merienda-one";
  text-align: center;
  font-size: 3em;
  color:lightseagreen;
  font-weight: 100;
}
h6{
    font-family: 'Dosis', "merienda-one";
  text-align: center;
  color:lightseagreen;
}
.row {
  background-color:#4cc1bb;
  background-size: cover;
  width: 420px;
  height: 500px;
  padding: 10px;
  margin: auto;
  border-top: 7px solid #e42148;
  margin-top:3%;
}

.form-p {
  padding: 5px;
}

input[type="email"],
input[type="password"] {
  display: block;
  width: 95%;
  border: none;
  border-bottom: solid 1px black;
  padding: 1em;
  font-size: 1.2em;
  color: black;
  margin: 35px 0 0;
  margin-left: 8px;
  outline: none;
  background-color: transparent;
  margin-top:15%;
}

input[type="button"] {
  font-size: 1em;
  background-color: #e42148;
  color: #fff;
  width: 100%;
  height: 50px;
  outline: none;
  border: none;
  margin-top:30%;
}

input[type="button"]:hover {
  background-color: #9f1732;
}

.form-p {
  font-size: 1.1em;
  color: #fff;
  letter-spacing: 3px;
  text-align: center;
}

.form-p a {
  color: #fff;
  text-decoration: none; 
}

.form-p a:hover {
  color: #9999ff;
}
</style>
