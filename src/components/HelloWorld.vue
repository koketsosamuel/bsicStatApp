<template>
  
  <div class="ui container">
    
    <h1>K Basic Stats</h1>
    <hr>
 

    <div class="ui message blue">Enter the values in your discrete data one by one, then press <b>done</b> when you are finished. [must have atleast three values]</div>
    
    <br>

    <div class="ui action input">
      <input type="number" placeholder="Enter Data" v-model="val" @keydown.enter="addVal">
      <button class="ui button" @click="addVal">Add</button>
    </div>

    <br>
    <br>

    <button class="ui button blue huge" @click="done">Done</button>
    <button class="ui button red huge" @click="data = []">Reset</button>

    <br>

    <h3 v-if="data.length >= 1">Sorted Data</h3>


    <div class="ui message blue" v-if="data.length >= 1">

      <span v-for="(value, index) in data" :key="index">{{value}}&nbsp;&nbsp; </span> 

    </div>

    <br>

    

  

    <br>


  <!-- MODAL -->
  <div class="ui basic modal">
    <div class="ui header">
      
      Data Analysis

    </div>
    <div class="content">

      <table class="ui celled table">
        <tbody class="ui blue">

          <tr class="">
            <td>Mean</td>
            <td>{{mean}}</td>
          </tr>

          <tr class="primary">
            <td>Mode</td>
            <td>{{mode}}</td>
         
          </tr>

          <tr>
            <td>Range</td>
            <td>{{range}}</td>
          </tr>

          <tr class="primary">
            <td>Minimum</td>
            <td>{{min}}</td>
          </tr>

          <tr class="primary">
            <td>Maximum</td>
            <td>{{max}}</td>
          </tr>

          <tr class="primary">
            <td>Median (Q2)</td>
            <td>{{q2}}</td>
          </tr>

          <tr class="primary">
            <td>Inter Quartile Range (IQR)</td>
            <td>{{iqr}}</td>
          </tr>

          <tr class="primary">
            <td>Data Skewness</td>
            <td>{{skew}}
             
              <span v-if="Number(skew) > 0">&nbsp; (right)</span>
              <span v-if="Number(skew) < 0">&nbsp; (left)</span>
              <span v-if="Number(skew) == 0">&nbsp; (symmetrical)</span>


            
            </td>
          </tr>

        </tbody>
      </table>

    </div>
    <div class="actions">
      <div class="ui red basic cancel inverted button">
        Close
      </div>
    </div>
  </div>



  </div>

</template>

<script>
import {min, mean, max, interquartileRange, quantile, median, sampleSkewness, mode, range} from 'simple-statistics'


export default {


  data() {

    return {

      val: "",
      data: [],
      mode: "",
      mean: "",
      skew: "",
      q2: "",
      iqr: "",
      min: "",
      max: "",
      range: ""

    }

  },

  methods: {

    addVal() {

      if (this.val != "") {
        this.data.push(Number(this.val))
        this.val = ""
        this.sort()
      }

    },

    done() {

      if (this.data.length > 2) {
        this.calc()

        $(document).ready(() => {
            $('.ui.basic.modal').modal('show')
        })

      }

    },

    calc() {

        
       if (this.data.length >= 1) {


        this.mean = mean(this.data)
        this.mode = mode(this.data)
        this.min = min(this.data)
        this.max = max(this.data)
        this.q2 = median(this.data)
        this.iqr = interquartileRange(this.data)
        this.range = this.max - this.min
        this.skew = sampleSkewness(this.data)


       }


    },

    sort() {

      this.data.sort(function(a, b){return a-b});

    }

  }


}
</script>

<style>
  * {
    transition: all 0.3s;
  }
</style>
