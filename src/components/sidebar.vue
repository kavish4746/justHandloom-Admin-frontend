<template>
  <div>
<div class="area"></div>
<nav class="main-menu">
            
            <ul>
                <li>
                    <router-link to="/dashboard">
                        
                        <span style="margin-left:10%;">
                            <img src="../images/jH.png" style="height:50%;width:50%;">
                        </span>
                    </router-link>
                </li>
                <li>
                    <router-link to="/dashboard">
                        <i class="fa fa-home fa-2x"></i>
                        <span class="nav-text">
                            Dashboard
                        </span>
                    </router-link>
                  
                </li>
                <li class="has-subnav">
                    <router-link to="/category">
                        <i class="fa fa-laptop fa-2x"></i>
                        <span class="nav-text">
                            Categories
                        </span>
                    </router-link>
                </li>
                <li class="has-subnav">
                    <router-link to="/profile">
                        <i class="fa fa-user-circle fa-2x"></i>
                        <span class="nav-text">
                            Profile
                        </span>
                    </router-link>
                </li>
                <li class="has-subnav">
                    <router-link to="/product">
                       <i class="fa fa-table  fa-2x"></i>
                        <span class="nav-text">
                        Products
                        </span>
                    </router-link>
                    
                </li>
                <li class="has-subnav">
                    <router-link to="/order">
                       <i class="fa fa-table  fa-2x"></i>
                        <span class="nav-text">
                        Orders
                        </span>
                    </router-link>
                   
                </li>
                <li>
                    <router-link to="/bill">
                        <i class="fa fa-list-alt  fa-2x"></i>
                        <span class="nav-text">
                            Generated Bills
                        </span>
                    </router-link>
                </li>
            </ul>

            <ul class="logout">
                <li>
                   <router-link to="/">
                         <i class="fa fa-power-off fa-2x"></i>
                        <span class="nav-text">
                            Logout
                        </span>
                   </router-link>
                </li>  
            </ul>
        </nav>
  </div>
</template>
<script>

</script>
<style scoped>
</style>
