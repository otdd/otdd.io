<template>
  <div class="container">
    <Header />
    <div>
      <div style="margin:60px auto 0px auto; padding:0px 0px 200px 0px; width:1000px;text-align:left;font-size:16px">
        <div style="font-size:24px;font-weight:bold;margin-bottom:20px">Why OTDD?</div>
        <div class="section-content">
          <div style="margin:10px 0px; font-weight:bold;">Development and Testing for microservices have three main troubles:</div>
          <div>
            1. Hard to setup the outbound dependencies<br>
            &nbsp;&nbsp;&nbsp;&nbsp;Many third-party dependent services must be setup or mocked, not to mention they can be in inconsistent state very likely.<br>
          </div>
          <div style="margin-top:10px;">
            2. Hard to construct complicate test cases<br>
            &nbsp;&nbsp;&nbsp;&nbsp;A complicate test case may involve construct the proper inbound request and more importantly many more the proper complicate outbound responses.
          </div>
          <div style="margin-top:10px;">
            3. Hard to maintain the test cases<br>
            &nbsp;&nbsp;&nbsp;&nbsp;A large number of test cases must be written to have high test coverage of the development code. And microservices tend to change quickly nowadays, new features are added in weeks in a typical scrum process. The test cases can be very hard to maintain.
          </div>
        </div>
        <div class="section-title">
          How OTDD Deal With These Troubles
        </div>
        <div class="section-content">
          <div>
            1. All outbound dependencies are mocked automatically using the recorded test case
          </div>
          <div style="padding:10px;">
            All outbound dependencies are mocked by OTDD. No need to worry about the dependent services's state.<br>
            Iptables rules are setup to redirect all outbound connections to local OTDD test runner. When a test is run, the test runner then fetches corresponding outbound response based on data recorded from online.
            <div style="font-weight:bold;">
              So the outbound dependencies can be very easy to setup or no need to setup them at all.
            </div>
          </div>
          <div style="margin-top:10px;">
            <div>
              2. Test cases are automatically recorded by OTDD rathar than written manually<br>
            </div>
            <div style="padding:10px;">
              A test case contains the inbound request/response and its <span style="font-weight:bold;">corresponding</span> outbound requests/responses. No need to write them manually, they are recorded automatically by OTDD.
              <div style="font-weight:bold;">
                So no matter how complicate a test case can be, it is recorded automcatically.
              </div>
            </div>
          </div>
          <div style="margin-top:10px;">
            <div>
              3. Test cases are recorded constantly and stored<br>
            </div>
            <div style="padding:10px;">
              Test cases are recorded constantly at the rate of one test case within one second approximately. <br>When the new features of microservices are online, the test cases of these new features will be recorded automcatically, making the whole test base envolve with the online code itself.
              <div style="font-weight:bold;">
                So it's not that nessassary to carefully maintain the test case base any more.
              </div>
            </div>
          </div>
          <Button to="/getstarted" type="primary" target="_blank" style="margin:40px 10px;">
            Get Started
          </Button>
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
