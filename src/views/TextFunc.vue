<template>
  <div v-for="(item, i) in data">
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
.button {
  font-family: Avenir, sans-serif;
  position: relative;
  background-color:#FFAB9D;
  border: none;
  font-size: 18px;
  text-align: center;
  padding: 20px;
  text-align: center;
  width: 10%;
  height: 3%;
  font-weight: bold;
  transition-duration: 0.4s;
  text-decoration: none;
  overflow: hidden;
  cursor: pointer;
  color: black;
}

.button:after {
  content: "";
  background: #f1f1f1;
  display: block;
  position: absolute;
  padding-top: 300%;
  padding-left: 350%;
  margin-left: -20px !important;
  margin-top: -120%;
  opacity: 0;
  transition: all 0.8s
}

.button:active:after {
  padding: 0;
  margin: 0;
  opacity: 1;
  transition: 0s
}
</style>