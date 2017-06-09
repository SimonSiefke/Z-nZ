<template>
  <div>
    <h1>Invertibles for Z/nZ</h1>
    <span>{{getAll}}</span>
    <main class="hello">
      <div id="options">
      <form @submit.prevent="">
        <label for="n">n = </label>
        <input id="n" type="number" v-model.number="number"/>
      </form>
      <form @submit.prevent="">
        <label for="filter">show:  </label>
         <select name="" id="filter" v-model="filter">
          <option value="all">all</option>
          <option value="invertible">invertible</option>
          <option value="non-invertible">non-invertible</option>
          <option value="regular">regular</option>
          <option value="zero divisor">zero divisor</option>
        </select>
      </form>
      </div>
      <div id="info">
      <table>
        <th :title="filter + ' elements in Z/' + number + 'Z'">element</th>
        <th :title="'elements b so that a * b % ' + number + ' = 1'">inverse</th>
        <th :title="'non-zero elements b so that a * b % ' + number + ' = 0'">zero divisor</th>   
        <tr v-if="filter==='all' " v-for="(inverted, element) in invertibles">
          <td>{{element}}</td>
 <td v-if='inverted' :title="'(' + element + ' * ' + inverted + ')' + ' % ' + number + ' = 1'">{{inverted}}</td>
          <td v-else>-</td>
          <td v-if='zeroDivisors[element]' 
              :title="'(' + element + ' * ' + zeroDivisors[element] + ')' + ' % ' + number + ' = 0'">{{zeroDivisors[element]}}
          </td>
          <td v-else>-</td>
        </tr>
        <tr v-if="filter==='invertible'" v-for="[inverted, element] in invertibles.map((x,index)=>[x,index]).filter(x=>!x[0])">
           <td>{{element}}</td>
 <td v-if='inverted' :title="'(' + element + ' * ' + inverted + ')' + ' % ' + number + ' = 1'">{{inverted}}</td>
          <td v-else>-</td>
          <td v-if='zeroDivisors[element]' 
              :title="'(' + element + ' * ' + zeroDivisors[element] + ')' + ' % ' + number + ' = 0'">{{zeroDivisors[element]}}
          </td>
          <td v-else>-</td>
        </tr>
        <tr v-if="filter==='non-invertible'" v-for="[inverted, element] in invertibles.map((x,index)=>[x,index]).filter(x=>x[0])">
           <td>{{element}}</td>
 <td v-if='inverted' :title="'(' + element + ' * ' + inverted + ')' + ' % ' + number + ' = 1'">{{inverted}}</td>
          <td v-else>-</td>
          <td v-if='zeroDivisors[element]' 
              :title="'(' + element + ' * ' + zeroDivisors[element] + ')' + ' % ' + number + ' = 0'">{{zeroDivisors[element]}}
          </td>
          <td v-else>-</td>
        </tr>
         <tr v-if="filter==='regular'" v-for="[zeroDivisor, element] in zeroDivisors.map((x,index)=>[x,index]).filter(x=>x[0])">
           <td>{{element}}</td>
 <td v-if='invertibles[element]' :title="'(' + element + ' * ' + invertibles[element] + ')' + ' % ' + number + ' = 1'">{{invertibles[element]}}</td>
          <td v-else>-</td>
          <td v-if='zeroDivisor' 
              :title="'(' + element + ' * ' + zeroDivisor + ')' + ' % ' + number + ' = 0'">{{zeroDivisor}}
          </td>
          <td v-else>-</td>
        </tr>
         <tr v-if="filter==='zero divisor'" v-for="[zeroDivisor, element] in zeroDivisors.map((x,index)=>[x,index]).filter(x=>!x[0])">
           <td>{{element}}</td>
 <td v-if='invertibles[element]' :title="'(' + element + ' * ' + invertibles[element] + ')' + ' % ' + number + ' = 1'">{{invertibles[element]}}</td>
          <td v-else>-</td>
          <td v-if='zeroDivisor' 
              :title="'(' + element + ' * ' + zeroDivisor + ')' + ' % ' + number + ' = 0'">{{zeroDivisor}}
          </td>
          <td v-else>-</td>
        </tr>
      </table>

      <div id="more-info">
        <p>number of invertibles: {{this.invertibles.filter(x => x).length}}</p>
        <p>number of non-invertibles: {{this.invertibles.filter(x => !x).length}}</p>
        <p>number of zero divisors: {{this.zeroDivisors.filter(x => x).length}}</p>
        <p>number of regulars: {{this.zeroDivisors.filter(x => !x).length}}</p>
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
      filter: 'all',
      invertibles: [],
      zeroDivisors: []
    }
  },
  methods: {
    isInvertible (x) {
      for (let i = 1; i < this.number; i++) {
        if ((i * x) % this.number === 1) {
          return i
        }
      }
      return false
    },
    isZeroDivisor (x) {
      for (let i = 1; i < this.number; i++) {
        if ((i * x) % this.number === 0) {
          return i
        }
      }
      return false
    }
  },
  computed: {
    getZeroDivisors () {
      const startTime = new Date()
      this.zeroDivisors = [...Array(this.number)].map((x, index) => this.isZeroDivisor(index))
      const endTime = new Date()
      console.log(`get zero divisors: ${Math.floor((endTime - startTime) / 1000)},${(endTime - startTime) % 1000} s`)
    },
    getAll () {
      this.getZeroDivisors
      this.getOnes
      const startTime = new Date()
      this.invertibles = [...Array(this.number)].map((x, index) => this.isInvertible(index))
      const endTime = new Date()
      console.log(`get all: ${Math.floor((endTime - startTime) / 1000)},${(endTime - startTime) % 1000} s`)
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
  #options{
    flex:1 0 100%;
  }
  form{
    display: inline-flex;
    padding-right: .75em;
  }
  
  #info{
    margin-top:15px;
    display: flex;
    flex:1;
  }
  input{
    width: 45px;
  }
  label{
    padding-right: .2em
  }
  #more-info{
    flex:1;
    padding-left:2em;
  }
  
  p{
    margin: .5em 0 0 0;
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
