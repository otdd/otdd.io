<template>
  <div class="container">
    <Header />
    <div style="margin:0px 140px;text-align:left;">
      <div style="font-size:24px;padding:40px 0px 40px 0px;">I'm proud to announce that the 0.1.0 release is officially released on 2020/3/21. <a target="_blank" href="https://github.com/otdd/release/archive/0.1.0.tar.gz">Download Here</a></div>
      <div style="font-size:24px;padding:10px 0px;">Release Note</div>
      <div style="font-size:24px;">Features:</div>
      <ul style="font-size:14px;padding:10px 20px;">
        <li>the first implementation of the otdd idea that inbound req/resp and outbound reqs/resps are recorded and grouped as tests and can be run in test/dev env.</li>
        <li>support for http module ( outbound protocols are not limited.)</li>
        <li>ddl support: http, thrift, redis</li>
      </ul>
      <div style="font-size:24px;">Istio Version Support:</div>
      <ul style="font-size:14px;padding:10px 20px;">
        <li>1.1.x</li>
        <li>1.2.x</li>
        <li>1.3.x</li>
        <li>1.4.x</li>
        <li>1.5.x</li>
        <li>0.x and 1.0.x versions are not supported</li>
      </ul>
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
</stype>
