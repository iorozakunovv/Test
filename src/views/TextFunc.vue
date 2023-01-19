<template>
  <body class="v">
    <div class="back">
      <div class="all"  v-for="(item, i) in data">
    <p>{{ item.question }}</p>
    <div class="variant" v-for="variant in item.variants">
      <input class="radio" type="radio" :name="item.id" :id="variant" :value="variant" v-model="check[i].value">
      <label class="var" :for="variant">{{ variant }}</label>
    </div>
    <p class="check" v-if="end">{{ result[i].checked ? 'Correct' : 'Incorrect' }}</p>
    <div>
      <p>{{}}</p>
    </div>
  </div>
  
  <button class="button" @click="send">Send</button>
  <div>

  </div>
    </div>
    
  </body>
 
 
</template>
<script>
export default {
  data: () => ({
   
    data: null,
    answer: null,
    picked: null,
    result: null,
    end: false

  }),
  methods: { 

    async getdata() {
      const response = await fetch('http://localhost:3000/questions')
      this.data = await response.json()
      this.check = this.data.map(item => ({
        value: null
      }));

    },
    async getanswer() {
      const response = await fetch('http://localhost:3000/answers')
      this.answer = await response.json()
    },
    send() {
      this.result = this.answer.map((item, i) => ({
        ...item,
        checked: this.check[i].value === item.answer
      }))
      this.end = true;
    }

  },
  mounted() {
    this.getanswer()
    this.getdata()

  }
}
  //have fun
</script>
<style scoped>
.back{
  background-color: rgb(212, 212, 212);
  border: 5px solid;
  margin: 20px;
}
.variant{
  color: black;
}
.var{
  color: rgb(23, 23, 23);
}
.check{
  color: rgb(0, 0, 0);
}
.all{
  margin-left: 13px;
}
p{
  color: rgb(22, 22, 22);
  font-size: 27px;
  font-weight: bold
}
.button{
  background-color: rgb(255, 192, 57);
   color: white;
   margin: 8px;
}
.button:hover{
 background-color: rgb(237, 123, 0);
 cursor: pointer;

}


.radio{
  color: white;
}

body.v{
    overflow: hidden;    
    -webkit-animation: v 8s infinite;
    animation: v 8s infinite;
}

@keyframes v{
  0% {background:#29ADF0;}
  20%{background:#F02999;}
  40%{background:#F06E29;}
  60%{background:#F0D829;}
  80%{background:#29F03A;}
  100%{background:#29ADF0;}
}

@-webkit-keyframes v{
  0% {background:#29ADF0;}
  20%{background:#F02999;}
  40%{background:#F06E29;}
  60%{background:#F0D829;}
  80%{background:#29F03A;}
  100%{background:#29ADF0;}
}
</style>