<template>
  <div id="app">
    <div class="row">
      <div class="col s12">
        <h4 class="center-align">Онлайн пример нейронной сети</h4>
      </div>
      <div class="col s12 m5">
        <div class="card-panel">
          <h5>Ввод значений:</h5>
          <table>
            <thead>
              <tr>
                <td>Значение 1</td>
                <td>Значение 2</td>
                <td>Значение 3</td>
                <td>Значение 4</td>
                <td>Результат</td>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(dataT, index) in dataTrain">
                <td><input type="text" v-model="dataT[0]"></td>
                <td><input type="text" v-model="dataT[1]"></td>
                <td><input type="text" v-model="dataT[2]"></td>
                <td><input type="text" v-model="dataT[3]"></td>
                <td><input type="text" v-model="dataTrainRes[index]"></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
      <div class="col s6 m3 data-test">
        <div class="card-panel">
          <h5>Данные для теста:</h5>
          <div class="row">
            <div class="col s8 input-field offset-s2">
              <input placeholder="" id="val0" type="text" v-model="dataTest[0]">
              <label for="val0">Первое значение</label>
            </div>
            <div class="col s8 input-field offset-s2">
              <input placeholder="" id="val1" type="text" v-model="dataTest[1]">
              <label for="val1">Второе значение</label>
            </div>
            <div class="col s8 input-field offset-s2">
              <input placeholder="" id="val2" type="text" v-model="dataTest[2]">
              <label for="val2">Третье значение</label>
            </div>
            <div class="col s8 input-field offset-s2">
              <input placeholder="" id="val3" type="text" v-model="dataTest[3]">
              <label for="val3">Четвертое значение</label>
            </div>
          </div>
        </div>
      </div>
      <div class="col s6 m4 data-test">
        <div class="card-panel">
          <h5>Результат вычисления:</h5>
          <p>{{resultTest}}</p>
          <button @click="brain" class="waves-effect waves-light btn">Вычислить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        dataTrain: [
          [0, 0, 1, 0],
          [0, 1, 1, 1],
          [1, 0, 0, 0],
          [1, 1, 1, 0],
        ],
        dataTrainRes: [0, 1, 1, 0],
        dataTest: [1, 0, 0, 0],
        resultTest: 0,
        net: {}
      }
    },
    mounted() {
      this.net = new brain.NeuralNetwork();
      this.$nextTick(function() {
        setTimeout(() => {
          Materialize.updateTextFields()
        }, 100)
      })
      this.brain()
    },
    methods: {
      brain() {
        let data = [];
        for (let i = 0; i < this.dataTrain.length; i++) {
          data.push({
            input: this.dataTrain[i].map((val, index) => {
              return val
            }),
            output: [this.dataTrainRes[i]]
          })
        }

        console.log(data)
        console.log(this.dataTest)
        this.net.train(data);

        this.resultTest = this.net.run(this.dataTest)[0];

      }
    }
  }
</script>

<style>
  #app {}
  
  .data-test h5 {
    margin-bottom: 25px;
  }
</style>