<template>
  <div style="width:100%; background-color:#458CE9">
    <img width=216 height=64 src="~/assets/logo.png" style="float: left; margin-left: 0px;">
    <div class="header-item" style="margin-right:100px"></div>
    <!--
    <div class="header-item">Documentation</div>
    -->
    <div class="header-item"><nuxt-link to="/getstarted" style="color:#F7F7F7;">Get Started</nuxt-link></div>
    <div class="header-item"><nuxt-link to="/whyotdd" style="color:#F7F7F7;">Why Otdd</nuxt-link></div>
    <div style="clear:both"></div>
  </div>
</template>
<style>
.NuxtLogo {
  animation: 1s appear;
}

@keyframes appear {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
.header-item{
  float:right;
  margin-right:20px;
  color:white;
  font-size:16px;
  line-height:64px;
}

.header-item a:hover{
  color:black;
}

</style>
