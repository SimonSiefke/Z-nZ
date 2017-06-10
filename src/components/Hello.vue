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
        <label for="currentFilter">show:  </label>
         <select name="" id="currentFilter" v-model="currentFilter">
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
        <th :title="currentFilter + ' elements in Z/' + number + 'Z'">element</th>
        <th :title="'elements b so that a * b % ' + number + ' = 1'">inverse</th>
        <th :title="'non-zero elements b so that a * b % ' + number + ' = 0'">zero divisor</th>
        <tr v-for="element in getCurrentElements">
          <td>{{element}}</td>
          <td>{{invertibles[element]||'-'}}</td>
          <td>{{zeroDivisors[element]||'-'}}</td>
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
      elements: [],
      invertibles: [],
      zeroDivisors: [],
      currentFilter: 'all'
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
    getElements () {
      this.elements = [...Array(this.number)].map((x, index) => index)
    },
    getZeroDivisors () {
      const startTime = new Date()
      this.zeroDivisors = this.elements.map(x => this.isZeroDivisor(x))
      const endTime = new Date()
      console.log(`get zero divisors: ${Math.floor((endTime - startTime) / 1000)},${(endTime - startTime) % 1000} s`)
    },
    getInvertibles () {
      const startTime = new Date()
      this.invertibles = this.elements.map(x => this.isInvertible(x))
      const endTime = new Date()
      console.log(`get invertibles: ${Math.floor((endTime - startTime) / 1000)},${(endTime - startTime) % 1000} s`)
    },
    getAll () {
      const startTime = new Date()
      this.getElements
      this.getInvertibles
      this.getZeroDivisors
      const endTime = new Date()
      console.log(`get all: ${Math.floor((endTime - startTime) / 1000)},${(endTime - startTime) % 1000} s`)
    },
    getCurrentElements () {
      switch (this.currentFilter) {
        case 'all':
          return this.elements
        case 'invertible':
          return this.invertibles.reduce((prev, x, index) => x ? prev.concat(index) : prev, [])
        case 'non-invertible':
          return this.invertibles.reduce((prev, x, index) => !x ? prev.concat(index) : prev, [])
        case 'regular':
          return this.zeroDivisors.reduce((prev, x, index) => !x ? prev.concat(index) : prev, [])
        case 'zero divisor':
          return this.zeroDivisors.reduce((prev, x, index) => x ? prev.concat(index) : prev, [])
      }
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
