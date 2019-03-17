<template>
  <section class="body">
    <div class="default">
      {{msg}} {{body}}<br>
      {{msg2}} {{body2}}
    </div>

    <div class="topic">
      {{msg3}} {{body3}}<br>
      {{msg4}} {{body4}}
    </div>

    <div class="order">
      {{msg5}} {{body5}}
    </div>

    <div class="trans">
      {{msg6}} {{body6}}
    </div>

    <div class="btnbody">
      <button class="btn" @click="t1">集群并发消息</button>
      <button class="btn" @click="t2">广播并发消息</button>
      <button class="btn" @click="t3">顺序消息</button>
      <button class="btn" @click="t4">事务消息</button>
      <button class="btn" @click="t5">确认回查状态</button>
      <button class="btn" @click="clear">清空</button>
    </div>
  </section>
</template>

<script>

  import axios from 'axios'

  export default {
    data() {
      return {
        msg: '集群队列1:',
        msg2: '集群队列2:',
        msg3: '广播队列1:',
        msg4: '广播队列2:',
        msg5: '顺序队列:',
        msg6: '事务队列:',
        body3: '',
        body4: '',
        body5: '',
        body: '',
        body2: '',
        body6: '',
        websock: null,
        websock2: null,
        websock3: null,
        websock4: null,
        websock5: null,
        websock6: null,
      }
    },
    mounted() {
      //页面刚进入时开启长连接
      this.initWebSocket2();
      this.initWebSocket();
      this.initWebSocket3();
      this.initWebSocket4();
      this.initWebSocket5();
      this.initWebSocket6();
    },
    methods: {
      initWebSocket() { //初始化weosocket
        this.websock = new WebSocket('ws://localhost:8081/default1');
        this.websock.onmessage = this.websocketonmessage;
      },
      websocketonmessage(e) { //数据接收
        this.body += e.data == '连接成功' ? '' : e.data + ' '
      },

      initWebSocket2() { //初始化weosocket
        this.websock2 = new WebSocket('ws://localhost:8083/default2');
        this.websock2.onmessage = this.websocketonmessage2;
      },
      websocketonmessage2(e) { //数据接收
        this.body2 += e.data == '连接成功' ? '' : e.data + ' '
      },

      initWebSocket3() { //初始化weosocket
        this.websock3 = new WebSocket('ws://localhost:8081/topic1');
        this.websock3.onmessage = this.websocketonmessage3;
      },
      websocketonmessage3(e) { //数据接收
        this.body3 += e.data == '连接成功' ? '' : e.data + ' '
      },

      initWebSocket4() { //初始化weosocket
        this.websock3 = new WebSocket('ws://localhost:8083/topic2');
        this.websock3.onmessage = this.websocketonmessage4;
      },
      websocketonmessage4(e) { //数据接收
        this.body4 += e.data == '连接成功' ? '' : e.data + ' '
      },


      initWebSocket5() { //初始化weosocket
        this.websock5 = new WebSocket('ws://localhost:8081/order');
        this.websock5.onmessage = this.websocketonmessage5;
      },
      websocketonmessage5(e) { //数据接收
        this.body5 += e.data == '连接成功' ? '' : e.data + ' '
      },


      initWebSocket6() { //初始化weosocket
        this.websock5 = new WebSocket('ws://localhost:8080/transaction');
        this.websock5.onmessage = this.websocketonmessage6;
      },
      websocketonmessage6(e) { //数据接收
        this.body6 += e.data == '连接成功' ? '' : e.data + ' '
      },


      clear(){
        this.body=''
        this.body2=''
        this.body3=''
        this.body4=''
        this.body5=''
        this.body6=''
      },

      t1(){
        axios.get("http://localhost:8080/default_order_send").then()
      },
      t2(){
        axios.get("http://localhost:8080/default2_order_send").then()
      },
      t3(){
        axios.get("http://localhost:8080/order_send").then()
      },
      t4(){
        axios.get("http://localhost:8080/transaction_send?str=测试").then()
      },
      t5(){
        axios.get("http://localhost:8080/add?a=1").then()
      },
    },
  }
</script>

<style scoped>
  .default {
    height: 100px;
    background: #6FA5DB;
    font-size: 30px;
    margin: 10px 0 0 0;
  }

  .topic {
    height: 100px;
    background: #9a9afb;
    font-size: 30px;
    margin: 40px 0 0 0;
  }

  .order {
    height: 100px;
    background: #006666;
    font-size: 30px;
    margin: 40px 0 0 0;
  }

  .trans {
    height: 100px;
    background: orangered;
    font-size: 30px;
    margin: 40px 0 0 0;
  }

  .btnbody {
    margin: 80px 0 0 420px;
  }

  .btn {
    background: cadetblue;
    color: white;
    font-size: 25px;
    width: 150px;
    height: 100px;
    /*margin: 0 50px 0 0;*/
    align-items: center;
    border-radius: 20px;
  }

  .body {
    margin: 10px 0 0 0;
  }
</style>
