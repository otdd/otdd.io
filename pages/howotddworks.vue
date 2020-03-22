<template>
  <div class="container">
    <Header />
    <div style="margin:0px 140px;text-align:left;padding-bottom:300px;">
      <div style="font-size:24px;font-weight:bold;padding:40px 0px 20px 0px;">There are two main problems need to be solved by OTDD</div>
      <ul style="font-size:14px;padding:10px 20px;">
         <li style="font-size:18px;">How Test Cases Are Recorded</li>
         <li style="font-size:18px;">How Test Cases Are Run</li>
      </ul>
      <div style="font-size:24px;font-weight:bold;padding:40px 0px 20px 0px;">How Test Cases Are Recorded</div>
      <div style="font-size:18px;font-weight:bold;padding:20px 0px 20px 0px;">The main goal of OTDD when recording test cases is to correctly group inbound req/resp with its outbound reqs/resps.</div>
      <div style="font-size:16px;padding:10px 0px;">Traditionally online inbound requests/responses and outbound requests/responses are very easy to collect, like usig a tcpdump, tcpdump or something else, but these inbound/outbound requests/responses are of little use if there are no correct relations between them.</div>
      <div style="font-size:16px;padding:10px 0px;">To group the inbound and outbound reqs/resps, there are three main methods:</div>
      <ul style="font-size:14px;padding:10px 20px;">
         <li>By using trace id</li>
         <li>By using thread id</li>
         <li>By using time gap</li>
      </ul>
      <div style="font-size:24px;padding:10px 0px;">By Using Trace ID</div>
      <div style="font-size:16px;padding:10px 0px;">This method utilize the trace id within the inbound req/resp and outbound reqs/resps to group them. But it has two main drawbacks:</div>
      <ul style="font-size:14px;padding:10px 20px;">
         <li>The trace id needs application layer to take care of the trace id as to make sure all outbound reqs/resps must carrys the id.</li>
         <li>Not all protocol provide a flexiable way to inject a trace id like a http header. e.g. the redis or mysql protocol.</li>
      </ul>
      <div style="font-size:24px;padding:10px 0px;">By Using Thread ID</div>
      <div style="font-size:16px;padding:10px 0px;">This method utilize the thread id of the inbound req/res and outbound reqs/resps to group them. But it also has two main drawbacks:</div>
      <ul style="font-size:14px;padding:10px 20px;">
         <li>Inbound req/resp and outbound reqs/resps are not necessarily reside in the same thread.</li>
         <li>Hijack code must be written and injected into the application in order to track the thread id.</li>
      </ul>
      <div style="font-size:24px;padding:10px 0px;">By Using Time Gap</div>
      <div style="font-size:16px;padding:10px 0px;">This method manages to arrange the inbound requests so that they get processed one bye one "serially", with a time gap between them. So within the time gap, the inbound req/resp and outbound reqs/resps are naturally grouped. It is the method OTDD choose to use.</div>
      <div style="font-size:16px;padding:10px 0px;">Here is how OTDD works:</div>
      <div style="font-size:16px;padding:10px 0px;">When OTDD is applied, two pods are created: the redirector and recorder. The redirector just looks like another normal pod that receive its requests from upstreams, while the recorder will receive no requests except only from the redirector. The redirector redirect the request at an interval, say 1 second, to the recorder, the recorder then groups the inbound req/resp and the outbound reqs/resps using the time gap.</div>
      <div style="font-size:16px;padding:10px 0px;">This method has the following main advantages:</div>
      <ul style="font-size:14px;padding:10px 20px;">
         <li>This method applys to most scenarios that most microservice fit in this modal.</li>
         <li>Recording happends at the tcp layer, no need for the application layer to do anything.</li>
         <li>With the help of kubernetes and istio, it makes the deployment very easy with only one command.</li>
      </ul>
      <div style="font-size:24px;font-weight:bold;padding:40px 0px 20px 0px;">How Test Cases Are Run</div>
      <div style="font-size:18px;font-weight:bold;padding:20px 0px 20px 0px;">The main goal of OTDD when running test cases is to correctly replay the inbound req/resp and outbound reqs/resps at tcp level.</div>
      <div style="font-size:16px;padding:10px 0px;">Most application layer protocols are based on tcp, replaying them at tcp level applys to most application protocols.</div>
      <div style="font-size:20px;font-weight:bold;padding:20px 0px 20px 0px;">The Test Runner</div>
      <div style="font-size:16px;padding:10px 0px;">The test runner fetches test cases constantly from the otddserver. When a test is specified to run from the otddserver's web page, it is fetched by the otdd test runner, the inbound request is sent by the runner to your running process, and all outbound requests are mocked by the runner using the response data of the test case. They all happen at the tcp level.</div>
      <div style="font-size:16px;padding:10px 0px;">There are two ways to run the test runner:</div>
      <ul style="font-size:14px;padding:10px 20px;">
         <li>Run as a standalone docker and attach your test/dev docker's network to it</li>
         <li>Run as a service in your linux box and attach your test/dev process to it</li>
      </ul>
      <div style="font-size:16px;font-weight:bold;padding:10px 0px;">Run as a standalone docker</div>
      <div style="font-size:16px;padding:10px 0px;">This is suitable when the code is developed in a docker. e.g. a php docker.</div>
      <div style="font-size:16px;font-weight:bold;padding:10px 0px;">Run as a service</div>
      <div style="font-size:16px;padding:10px 0px;">This is suitable when the code is developed in a linux box. </div>
      <div style="font-size:20px;font-weight:bold;padding:20px 0px 20px 0px;">The DDL Language</div>
      <div style="font-size:16px;padding:10px 0px;">In order to view and edit the tcp raw data, the DDL language is introduced for each application protocol.</div>
      <div style="font-size:16px;padding:10px 0px;">The DDL language and the corresponding DDL encoder/decoder plugins are responsible of:</div>
      <ul style="font-size:14px;padding:10px 20px;">
         <li>Decoding the raw tcp data into humman readable DDL content, which can then be edited</li>
         <li>Encoding the DDL content back into raw tcp data</li>
      </ul>
      <div style="font-size:16px;padding:10px 0px;">The DDL encoder/decoder plugins can be managed as to install or uninstall at the otddserver's manage page.</div>
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
