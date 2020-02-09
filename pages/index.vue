<template>
  <div class="container">
    <Header />
    <div>
      <div style="margin:120px auto 0px auto; width:1260px;">
        <div class="card">
          <div class="card-title">
            For Fast Development And Testing
          </div>
          <div class="card-content">
            Otdd is born for <span class="weight-content">fast development and testing</span> for microservices. <br><span class="weight-content">All inbound/outbound traffics are recorded</span> by otdd, <span class="weight-content">no need to</span> use a postman, curl or browser to construct the complex inbound request and <span class="weight-content">no need to</span> use some mock servers to mock the outbound dependencies any more.
          </div>
        </div>
        <div class="card">
          <div class="card-title">
            Easy To Use
          </div>
          <div class="card-content">
            Tests are <span class="weight-content">recorded online automatically</span> rather than written manually. You don't need to do anything just enjoy using them.
          </div>
        </div>
        <div class="card">
          <div class="card-title">
            Easy To Deploy
          </div>
          <div class="card-content">
            Otdd runs on istio and kubernetes. It can be <span class="weight-content">installed by a few commands</span>.
          </div>
        </div>
        <div class="card" style="margin-right:0px">
          <div class="card-title">
            Based On the TDD Idea
          </div>
          <div class="card-content">
            Otdd is based on the TDD idea applying on <span class="weight-content">microservices</span> specifically utilizing <span class="weight-content">online</span> recorded traffic.
          </div>
        </div>
      </div>
      <div style="clear:both" />
      <div class="links">
        <Button to="/getstarted" type="primary" target="_blank" style="margin-right:10p;">
          Get Started
        </Button>
        <!--
        <Button target="_blank" style="margin-right:10px" to="https://github.com/nuxt/nuxt.js">
          Documentation
        </Button>
        -->
        <Button target="_blank" style="margin-right:10px" to="https://github.com/otdd">
          Github
        </Button>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'
export default {
  components: {
    Header, Footer
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-color:#F7F7F7;
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
}
.card{
  float:left;
  width:300px;
  padding:0px;
  margin-right:20px;
}

.card-title{
  font-size:18px;
  margin-bottom:20px;
  font-weight:bold;
}

.card-content{
  font-size:16px;
  margin-bottom:20px;
}

.weight-content{
  font-weight:bold;
  color:#2d8cf0;
}

.links {
  padding-top: 40px;
}
</style>
