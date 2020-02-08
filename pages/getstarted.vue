<template>
  <div class="container">
    <Header />
    <div>
      <div style="margin:60px auto 0px auto; padding:0px 0px 100px 0px; width:1000px;text-align:left;font-size:16px">
        <div style="font-size:24px;font-weight:bold;margin-bottom:20px">Get Started</div>
        <div class="section-content">
          To get started with otdd, just follow these three steps:<br>1. Set up your platform<br>2. Download the release<br>3. Install Otdd
        </div>
        <div class="section-title">
          Set up your platform
        </div>
        <div class="section-content">
        Before you can install otdd, you need a cluster running a compatible version of <a href="https://kubernetes.io/" target="_blank">Kubernetes</a> and <a href="https://istio.io/docs/setup/getting-started/" target="_blank">Istio</a>. And make sure the istio is setup in auto injection mode. i.e. your namespace is labeled: <span style="font-weight:bold">istio-injection=enabled</span><br>Please refer to the table below to choose the versions to install.
        </div>
        <table class="section-table">
          <thead>
            <tr>
              <td>otdd version</td>
              <td>istio version</td>
              <td>kubernetes version</td>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>0.1.0</td>
              <td>1.2.2</td>
              <td>1.3</td>
            </tr>
          </tbody>
        </table>
        <div class="section-title">
          Download the release
        </div>
        <div class="section-content">
          Download the otdd release which includes installation files, and the otddctl command line utility.
          <div>
          1. Go to the link of the otdd release page at github and download the release source code. <a href="https://github.com/otdd/release/releases">github release page</a> <br>Or you can download it directly using following commands:
            <div class="section-code">
              $ version=0.1.0; wget -O release-$version.tar.gz https://github.com/otdd/release/archive/$version.tar.gz && tar -xzf release-$version.tar.gz && rm -f release-$version.tar.gz && mv release-$version otdd-$version
            </div>
          </div>
          <div>
          2. Move to the otdd package directory. For example, if the package is otdd-0.1.0:
            <div class="section-code">
              $ cd otdd-0.1.0
            </div>
          The directory contains:<br> &nbsp;&nbsp;&nbsp;&nbsp;1. The install directory: cotains install.sh/uninstall.sh to install/uninstall YAML files for Kubernetes<br>&nbsp;&nbsp;&nbsp;&nbsp;2.  The otddctl.sh command line tool. <br>&nbsp;&nbsp;&nbsp;&nbsp;3.  The template directory: contains a template file for otddctl.sh to use.
          </div>
        </div>
        <div class="section-title">
          Install Otdd
        </div>
        <div class="section-content">
          1. Go to the install directory.
          <div class="section-code">
            $ cd otdd-0.1.0/install
          </div>
          2. Run the install script.
          <div class="section-code">
            $ sh install.sh
          </div>
          3. Check whether otdd is successfully installed.
          <div class="section-code">
            $ kubectl -n otdd-system get pods
          </div>
          You should see three pods running:<br>
          &nbsp;&nbsp;&nbsp;&nbsp;1. <span style="font-weight:bold">otdd-adapter</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For handling istio mixer events. The recorded test cases are sent through mixer.<br>
          &nbsp;&nbsp;&nbsp;&nbsp;2. <span style="font-weight:bold">otdd-controller</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For redirector/recorder creation.<br>
          &nbsp;&nbsp;&nbsp;&nbsp;3. <span style="font-weight:bold">otdd-server</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The otdd web and the grpc server for test runner.<br>
        </div>
        <div class="section-title">
          Applying Otdd
        </div>
        <div class="section-content">
          1. Apply otdd to a target deployment. <br>&nbsp;&nbsp;&nbsp;&nbsp;Assume you have installed the official istio bookinfo sample, and you want to use otdd to help the development and testing of reviews-v2 app. (We choose reviews-v2 because it has the outbound dependency of ratings-v1 app)
          <div class="section-code">
            $ #option -t is for target deployment. -p is for target deployment's container port. <br>
            $ #please run "otddctl.sh help" to see more options.<br>
            $ ISTIO_VERSION="1.2.2"; sh otddctl.sh -v $ISTIO_VERSION -t reviews-v2 -p 9080
          </div>
          &nbsp;&nbsp;&nbsp;&nbsp;Please make sure the ISTIO_VERSION is correct. otdd will install the redirector/recorder, and will substitute istio/proxyv2 to otdd compiled otdd/proxyv2 for them, if you specified the wrong version, the otdd/proxyv2 may not fit into your existing istio system.
        </div>
        <div class="section-content">
          2. Make some requests to the target deployment to let otdd record the test.<br> &nbsp;&nbsp;&nbsp;&nbsp; Let's take the bookinfo as an example, you can open the browser and refresh the bookinfo page several times to make some requests to reviews-v2. (As the requests are load-balanced for 3 reviews versions, v1, v2, v3, please refresh the browser better for 10+ times to make plenty of requests)
        </div>
        <div class="section-content">
          3. Open your browser to visit the web front of otddserver.<br>
          you should see a module named reviews-v2.default is created automatically and the tests are recorded already.
        </div>
        <div class="section-title">
          Using Otdd For Development And Testing
        </div>
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
  },
  methods: {
    getStarted () {
      window.open('#getstarted/')
    }
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

.weight-content{
  font-weight:bold;
  color:#2d8cf0;
}

.section-title{
  font-size:24px;
  font-weight:bold;
  margin-top:20px;
}

.section-content{
  line-height:32px;
  margin:20px 0px;
}

.section-table{
   border:1px solid #ccc;
   border-collapse: collapse;
}

.section-table td{
   border:1px solid #ccc;
   padding:8px 20px;
   text-align:center;
}

.section-code{
  padding:10px 20px;
  margin:10px;
  background-color: #535A6D;
  color:#F7F7F7;
  font-size:14px;
}

.links {
  padding-top: 40px;
}
</style>
