<template>
  <div class="container">
    <Header />
    <div>
      <div style="margin-top:40px;">
        <div style="margin-left:140px;width:620px;text-align:left;float:left;">
          <div style="font-size:18px;font-weight:bold;">OTDD is an open-source system to record traffic online and replay them in dev or test environment.</div>
          <div style="font-size:16px;margin-top:10px;">
            Pacifically it manages to group the inbound request/responses with its corresponding outbound requests/responses thus making it extremely useful as to:
          </div>
          <div style="margin-top:10px;font-size:16px;">
            1. examine the online cases<br>
            2. fasten the development by mocking all its dependencies<br>
            3. test the dev code using the real online cases<br>
          </div>
        </div>
        <img width=400 height=160 src="~/assets/logo_transparent.png" style="float: left; margin: 20px 40px;">
        <div style="clear:both;"></div>
      </div>
      <div style="margin:30px auto 0px auto; width:1260px;">
        <div class="card">
          <div class="card-title">
            For Fast Development And Testing
          </div>
          <div class="card-content">
            Otdd is born for <span class="weight-content">fast development and testing</span> for microservices. <br>
            <span class="weight-content">no need to</span> use a postman, curl or browser to construct the complex inbound request and <span class="weight-content">no need to</span> use some mock servers to mock the outbound dependencies any more.
          </div>
        </div>
        <div class="card">
          <div class="card-title">
            Easy To Use
          </div>
          <div class="card-content">
            Tests are <span class="weight-content">recorded online automatically</span> rather than written manually. No need to do anything just enjoy using them.
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
        <Button to="/whyotdd" type="primary" target="_blank" style="margin-right:10p;">
          Why Otdd
        </Button>
        <Button to="/getstarted" type="primary" target="_blank" style="margin-right:10p;">
          Get Started
        </Button>
        <Button target="_blank" type="primary" style="margin-right:10px" to="https://github.com/otdd">
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
  margin-top: 0px;
}
</style>
