<template>
  <div class="container">
    <Header />
    <div>
      <div style="margin:60px auto 0px auto; padding:0px 0px 200px 0px; width:1000px;text-align:left;font-size:16px">
        <div style="font-size:24px;font-weight:bold;margin-bottom:20px">Get Started</div>
        <div class="section-content">
          To get started with otdd, just follow these six steps:<br>1. Set up your platform<br>
          2. Download the release<br>
          3. Install Otdd<br>
          4. Apply Otdd To Existing Deployments<br>
          5. Setup The Otdd Test Runner For Development And Testing<br>
          6. Use The Otdd Web To Choose Tests To Run<br>
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
              <td>1.15</td>
            </tr>
          </tbody>
        </table>
        <div class="section-title">
          Download the release
        </div>
        <div class="section-content">
          Download the otdd release which includes installation files, and the otddctl command line utility.
          <div>
          1. Go to the link of the otdd release page at github and download the release source code. <a href="https://github.com/otdd/release/releases" target="_blank">github release page</a> <br>Or you can download it directly using following commands:
            <div class="section-code">
              $ OTDD_VERSION=0.1.0; wget -O release-$OTDD_VERSION.tar.gz https://github.com/otdd/release/archive/$OTDD_VERSION.tar.gz && tar -xzf release-$OTDD_VERSION.tar.gz && rm -f release-$OTDD_VERSION.tar.gz && mv release-$OTDD_VERSION otdd-$OTDD_VERSION
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
          2. Run the install script. Before install, please make sure the <a href="https://stedolan.github.io/jq/download/" target="_blank">jq</a> command line is installed. For reference, you can install it in centos 7 as follows:
          <div class="section-code">
            yum install epel-release -y<br>
            yum install jq -y
          </div>
          <div>
            Then execute the install.sh script.
          </div>
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
          Apply Otdd To Existing Deployments
        </div>
        <div class="section-content">
          1. Apply otdd to a target deployment. <br>&nbsp;&nbsp;&nbsp;&nbsp;Assume you have installed the official istio bookinfo sample, and you want to use otdd to help the development and testing of reviews-v2 app. (We choose reviews-v2 because it has the outbound dependency of ratings-v1 app)
          <div class="section-code">
            # option -t is for target deployment. -p is for target deployment's container port. <br>
            # please run "otddctl.sh help" to see more options.<br>
            $ ISTIO_VERSION="1.2.2"; sh otddctl.sh -v $ISTIO_VERSION -t reviews-v2 -p 9080
          </div>
          &nbsp;&nbsp;&nbsp;&nbsp;Please make sure the ISTIO_VERSION is correct. otdd will install the redirector/recorder, and will substitute istio/proxyv2 to otdd compiled otdd/proxyv2 for them, if you specified the wrong version, the otdd/proxyv2 may not fit into your existing istio system.
        </div>
        <div class="section-content">
          2. Make some requests to the target deployment to let otdd record the test.<br> &nbsp;&nbsp;&nbsp;&nbsp; Let's take the bookinfo as an example, you can open the browser and refresh the bookinfo page several times to make some requests to reviews-v2. (As the requests are load-balanced for 3 reviews versions, v1, v2, v3, please refresh the browser better for 10+ times to make plenty of requests)
        </div>
        <div class="section-content">
          3. Open your browser to visit the web front of otddserver.<br>
          &nbsp;&nbsp;&nbsp;&nbsp;You should see a module named reviews-v2.default is created automatically and the tests are recorded already.<br>
          &nbsp;&nbsp;&nbsp;&nbsp; <span style="font-weight:bold;">Please note</span>: The otdd-server is exposed as a kubernetes service of LoadBalancer type, exporting 8080 for the web and 8764 for the grpc ( used for the test runner running at your development node to fetch test case and receive test reports ), please refer to your cloud provider on how to configure the loadbalancer to access it from outside.<br>
          &nbsp;&nbsp;&nbsp;&nbsp;If you are using a minikube, you can run the following commands to see which port is mapped to locally.
          <div class="section-code">
            $ kubectl get svc -n otdd-system
          </div>
          &nbsp;&nbsp;&nbsp;&nbsp;For example, you may see the result is:
          <div class="section-code">
            $ otdd-server    LoadBalancer   10.96.26.60      &lt;pending&gt;    8764:<span style="color:#4EA06B">31427</span>/TCP,8080:<span style="color:#4EA06B">31478</span>/TCP   16m
          </div>
          &nbsp;&nbsp;&nbsp;&nbsp;Then find your minikube's ip:
          <div class="section-code">
            $ minikube status
          </div>
          &nbsp;&nbsp;&nbsp;&nbsp;You may see the result is:
          <div class="section-code">
            $ kubectl: Correctly Configured: pointing to minikube-vm at 172.16.75.130
          </div>
          &nbsp;&nbsp;&nbsp;&nbsp;Then you can visit the otdd web using a browser at: http://172.16.75.130:31478<br>
          &nbsp;&nbsp;&nbsp;&nbsp;The otdd grpc server is at: http://172.16.75.130:31427 (will be used in following section.)
        </div>
        <div class="section-title">
          Setup The Otdd Test Runner For Development And Testing
        </div>
        <div class="section-content">
          <div>Now all are setup, let's setup the otdd test runner for our development and testing!</div>
          <div style="font-size:16px;font-weight:bold;margin:0px 0px 20px 0px;">
            1. If Your Development Code is Run On Docker
          </div>
          <div>
            &nbsp;&nbsp;&nbsp;&nbsp;If your development code is run on a docker, e.g. a php/ruby/nginx docker image, then firstly run the otdd test runner docker as follows.
          </div>
          <div class="section-code">
            $ OTDD_VERSION=0.1.0; docker run --cap-add=NET_ADMIN --cap-add=NET_RAW -e OTDD_SERVER_HOST='<span style="color:#4EA06B">172.16.75.130</span>' -e OTDD_SERVER_PORT='<span style="color:#4EA06B">31427</span>' -e USERNAME='<span style="color:#4EA06B">yipjie</span>' -e TAG='<span style="color:#4EA06B">reviews-v2</span>' -d <span style="color:#4EA06B">-p 9080:9080</span> --name otdd-test-runner otdd-test-runner:$OTDD_VERSION<br>
          </div>
          <div class="code-explain">
            <table class="test-runner-exp">
              <tr>
                <td class="exp-name">
                  OTDD_SERVER_HOST :
                </td>
                <td>
                  the otdd server's port. In the minikube example above, it's 172.16.75.130;
                </td>
              </tr>
              <tr>
                <td class="exp-name">
                  OTDD_SERVER_PORT :
                </td>
                <td>
                  the otdd server's port. In the minikube example above, it's 31427;
                </td>
              </tr>
              <tr>
                <td class="exp-name">
                  USERNAME :
                </td>
                <td>
                  it's used for otdd server to distinguish from other users.
                </td>
              </tr>
              <tr>
                <td class="exp-name">
                  TAG:
                </td>
                <td>
                  it's used for yourself to distinguish from other apps. details-v1 / ratings-v1 for example.
                </td>
              </tr>
              <tr>
                <td class="exp-name">
                  -p :
                </td>
                <td>
                  <span style="font-weight:bold">care must be taken here</span>, the port in your docker must be exposed here firstly as your docker will share the test runner docker's network.
                </td>
              </tr>
            </table>
          </div>
          <div>
            &nbsp;&nbsp;&nbsp;&nbsp;After the test runner docker is run, run your docker like the following command.
          </div>
          <div class="section-code">
            $ docker run -d -e RATINGS_HOSTNAME=127.0.0.2 <span style="color:#4EA06B">--net=container:otdd-test-runner</span> --name examples-bookinfo-reviews-v2 docker.io/istio/examples-bookinfo-reviews-v2:1.12.0
          </div>
          <div>
            &nbsp;&nbsp;&nbsp;&nbsp;The <span style="font-weight:bold;">--net=container:otdd-test-runner</span> is used to set the network mode as container mode to attach to the runner.<br>
            &nbsp;&nbsp;&nbsp;&nbsp;The RATINGS_HOSTNAME is specific for the reviews-v2 app. If you are using another app to test, you can ignore it.
          </div>
          <div>
            <div style="font-size:16px;font-weight:bold;margin:32px 0px 32px 0px;">&nbsp;&nbsp;Understanding what happened.</div>
            <div style="margin:10px;padding:10px;border: 1px solid #ccc">
              1. When the test runner docker starts, it sets the iptables rules that will <span style="font-weight:bold;">intercept all outbound connections</span> back to the test runner. <br>
              2. The test runner docker then fetches test cases constantly from the otdd server. <br>
              3. When a test is fetched, the <span style="font-weight:bold;">inbound request is sent by the test runner</span> directly to the development docker.<br>
              4. As the development docker <span style="font-weight:bold;">share its network with the test runner container</span>, all of its outbound requests will be redirected to the test runner, the test runner then <span style="font-weight:bold;">fetches the corresponding response based on the online test</span> from the otdd server and sent it back to the development docker, thus <span style="font-weight:bold;">making the development docker effectively run as if it's in the online environment</span>.
            </div>
          </div>
          <div style="font-size:16px;font-weight:bold;margin:32px 0px 20px 0px;">
            2. If Your Development Code is Run On Linux Directly
          </div>
          <div style="padding-left:20px;">
            If your development code is run on linux directly, then you need to build the test runner and setup the envorionment manually. Below is the example commands for centos 7, you can change them based on different linux distributions. Some commands may need to be run as root.
            <div style="padding-top:20px;">
              <div>1. create two groups, otdd-test-runner and otdd.</div>
              <div class="section-code">
                # groupadd otdd; groupadd otdd-test-runner
              </div>
              <div>2. create a user named otdd-test-runner in the otdd-test-runner group.</div>
              <div class="section-code">
                # useradd otdd-test-runner -g otdd-test-runner
              </div>
              <div>3. build the test runner binary in the otdd-test-runner user. download the correct version of otdd-test-runner source code from <a href="https://github.com/otdd/otdd-test-runner/releases" target="_blank">github</a> and extract them into the /home/otdd-test-runner/go/src/otdd-test-runner directory</div>
              <div class="section-code">
                $ GOPATH=/home/otdd-test-runner/go/; cd $GOPATH/src/otdd-test-runner; go get ./... ; go build
              </div>
              <div>4. setup the iptables rules.</div>
              <div class="section-code">
                # iptables -t nat -A OUTPUT -p tcp -m owner --uid-owner otdd-test-runner -j ACCEPT<br>
                # iptables -t nat -A OUTPUT -p tcp -m owner --gid-owner otdd -j REDIRECT --to-port 18746
              </div>
              <div>5. start the test runner in the otdd-test-runner user</div>
              <div class="section-code">
                $ OTDD_SERVER_HOST="172.16.75.130"<br>
                $ OTDD_SERVER_PORT="31427"<br>
                $ USERNAME="yipjie"<br>
                $ TAG="reviews-v2"<br>
                $ cd $GOPATH/src/otdd-test-runner<br>
                $ nohup ./otdd-test-runner -h $OTDD_SERVER_HOST -p $OTDD_SERVER_PORT -u $USERNAME -t $TAG &
              </div>
              <div>6. start your development code normally but in the otdd group. take reviews app as an example, the command to start the reviews app is './server run reviews', then please start it as follows</div>
              <div class="section-code">
                $ sg otdd './server run reviews'
              </div>
            </div>
          </div>
          <div>
            <div style="font-size:16px;font-weight:bold;margin:32px 0px 32px 0px;">&nbsp;&nbsp;Understanding what happened.</div>
            <div style="margin:10px;padding:10px;border: 1px solid #ccc">
              <div style="margin:10px 0px;">The major differences between using docker are as follows.</div>
              <div style="margin:0px 0px 10px 0px;">1. The iptables rules that will intercept all outbound connections <span style="font-weight:bold;">from group otdd</span> to the test runner. In order to avoid the loop redirection, the outbound connections of user otdd-test-runner will passthrough.</div>
              <div style="margin:0px 0px 10px 0px;">2. The development code is run <span style="font-weight:bold;">in group otdd</span> using sg command if you want the outbound connections be mocked.</div>
              <div style="margin:0px 0px 10px 0px;">
                <span style="font-weight:bold;">Using a special otdd group is intentionally designed.</span> Because there may have lots of other processes running in your development node, there is no need to mass up with them. In stead, you have full control of whether or not to put your code into the otdd group for the test runner.
              </div>
            </div>
          </div>
        </div>
        <div class="section-title">
          Use The Otdd Web To Choose Tests To Run
        </div>
        <div class="section-content">
          <div>After all the above are setup, the tests are recorded automatically and the the test runner is fetching the tests to run constantly. All you need to do now is to choose some tests to run!</div>
          <div>
          1. module is automatically created.
          </div>
          <img width=1000 src="~/assets/otdd_web_modules.png" style="margin:10px;">
          <div>
          2. tests are automatically recorded. and you can choose the tests and the corresponding test runner to run.
          </div>
          <img width=1000 src="~/assets/otdd_web_testcase.png" style="margin:10px;">
          <div>
          3. after the tests are run, the reports are generated.
          </div>
          <img width=1000 src="~/assets/otdd_web_test_run_report.png" style="margin:10px;">
          <div>
          4. you can view the log.
          </div>
          <img width=1000 src="~/assets/otdd_web_test_run_log.png" style="margin:10px;">
          <div>
          5. you can find out the diff between the recorded outbound requests and actual outbound requests, and between the recorded inbound response and actual inbound response to find bugs.
          </div>
          <img width=1000 src="~/assets/otdd_web_test_run_report_inbound_resp_diff.png" style="margin:10px;">
          <img width=1000 src="~/assets/otdd_web_test_run_report_outbound_req_diff.png" style="margin:10px;">
          <div>
          6. you can edit an existing test. mock some new outbound calls to faliciate your development.
          </div>
          <img width=1000 src="~/assets/otdd_web_edit_test.png" style="margin:10px;">
          <div style="font-weight:bold;">
            There is much more powerfully and intresting feature of otdd, please feel free to enjoy it!
          </div>
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

.test-runner-exp .exp-name{
  text-align:center;
  width:200px;
}

.test-runner-exp td{
  border: 1px solid #ccc;
  padding:4px 10px;
}

.test-runner-exp{
  border-collapse: collapse;
  margin:10px;
}

.links {
  padding-top: 40px;
}
</style>
