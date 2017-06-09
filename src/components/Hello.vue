<template>
<div>
    <h1>Invertibles for Z/nZ</h1>
<span>{{getAll}}</span>
  <main class="hello">
   <select name="" id="" v-model="filter">
    <option value="all">all</option>
    <option value="invertible">invertible</option>
    <option value="non-invertible">non-invertible</option>
  </select>
<form @submit.prevent="">
    <label for="include0">include 0 </label>
    <input id="include0" type="checkbox" v-model.boolean="include0"/>
  </form> 
  <form @submit.prevent="">
    <label for="n">n = </label>
    <input id="n" type="number" v-model.number="number"/>
  </form>
  <div id="info">
  <table>
    <th>element</th>
    <th>inverse</th>
    <tr v-if="this.include0">
      <td>0</td>
      <td>-</td>
    </tr>
     
    <tr v-if="filter==='all' " v-for="(inverted, element) in pairs">
      <td>{{element}}</td>
      <td>{{inverted}}</td>`
    </tr>
    <tr v-if="filter==='invertible'" v-for="[inverted, element] in pairs.map((x,index)=>[x,index]).filter(x=>x[0]!=='-')">
      <td>{{element}}</td>
      <td>{{inverted}}</td>
    </tr>

    <tr v-if="filter==='non-invertible'" v-for="[inverted, element] in pairs.map((x,index)=>[x,index]).filter(x=>x[0]==='-')">
      <td>{{element}}</td>
      <td>{{inverted}}</td>
    </tr>
  </table>

  <div id="more-info">
  <p>number of invertibles: {{this.pairs.filter((x,index) => x[index + 1] !== '-').length}}</p>
  <p>number of non-invertibles: 
  {{
    this.pairs.filter((x,index) => x[index + 1] === '-').length + this.include0
  }}
  </p>
  <p>invertibles: {{this.pairs.filter((x,index) => x[index + 1] !== '-')}}</p>
</div>
 </div>
  
  </main>
  </div>

</template>

<script>
const app = {
  name: 'hello',
  data () {
    return {
      number: 4,
      arr: [1, 5, 9],
      include0: false,
      filter: 'all',
      pairs: []
    }
  },
  methods: {
    test (x) {
      console.log('test')
      for (let i = 0; i <= this.number; i++) {
        if (this.arr.includes(x * i)) {
          return i
        }
      }
      return '-'
    }
  },
  computed: {
    getNumbers () {
      console.log('getnumbers')
      let arr = [1]
      for (let i = 1; i < this.number; i++) {
        arr.push(i * this.number + 1)
      }
      this.arr = arr
    },
    getAll () {
      this.getNumbers
      console.log('getall')
      let all = new Array(this.number)
      for (let i = 0; i < this.number; i++) {
        all[i] = this.test(i)
      }
      this.pairs = all
    }
  }
}
export default app
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1{
    text-align: center;
  }
  main{

    display: flex;
    flex-flow: row wrap;
    width: 80%;
    margin: 0 auto;
  }
  form{
    flex:1 0 100%;
  }
  #info{
    margin-top:15px;
    display: flex;
    flex:1;
  }
  input{
    align-self: flex-start;
    width: 45px;
  }
  #more-info{
    flex:1;
    padding-left:2em;
    border: 2px solid green;
  }
  
  p{
    margin: .5em 0 0 0;
  }
  select{
    align-self: flex-start;
  }
  table{
    border: 1px solid black;
    display: inline-block;
  }
  tr:nth-child(odd){
    background-color:#ddeaff;
  }
  td{
    text-align:center;
  }
</style>
