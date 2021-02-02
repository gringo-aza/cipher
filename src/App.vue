<template>
  <div id="app" class="container mx-auto mt-4">
    <b-container class="mt-4">
      <b-row>
        <b-col lg="6">
          <label for="textarea1">
            <h2>Шифр Цезаря</h2>
          </label>
          <b-form-textarea
                  id="textarea1"
                  size="lg"
                  rows="5"
                  placeholder="Напишите текст чтобы зашифровать его"
                  v-model="text"
          ></b-form-textarea>

        </b-col>
        <b-col lg="6">

          <label for="textarea2"><h2>Результат</h2></label>
          <b-form-textarea
                  id="textarea2"
                  size="lg"
                  rows="5"
                  disabled
                  v-model="textCipher"
          ></b-form-textarea>

        </b-col>
      </b-row>

      <b-row class="mt-4">
        <b-col lg="3">
          <b-form-select v-model="selected"
                         :options="options">

          </b-form-select>
          <h4 class="mt-3">Сдвиг: <strong>{{ selected }}</strong></h4>
        </b-col>
        <b-col class="" lg="2">
          <b-button @click="encrypt" variant="primary">Зашифровать</b-button>
        </b-col>

        <b-col class="" lg="2">
          <b-button @click="decrypt" variant="success">Расшифровать</b-button>
        </b-col>
      </b-row>

    </b-container>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'


export default {
  name: 'App',
  data() {
    return {
      selected: null,
      text:'',
      textCipher:'',
      alphabet:'АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯабвгдеёжзийклмнопрстуфхцчшщъыьэюя',
      alphabetCipher:''
    }
  },
  computed: {
    options() {
      let options = [{value: 'null', text: 'Сдвиг'}];

      for (let i = 1; i <= 33; i++) {
        options.push({value: i, text: i});
      }
      return options;
    }
  },
  methods:{
    encrypt(){
      this.alphabetCipher = this.alphabet;
      this.alphabetCipher = this.shiftArray(this.alphabetCipher.split(''), this.selected).join('');

      this.textCipher = this.cryptText(this.text, this.alphabet, this.alphabetCipher);
    },
    decrypt(){
      this.alphabetCipher = this.alphabet;
      this.alphabetCipher = this.shiftArray(this.alphabetCipher.split(''), this.selected).join('');

      this.textCipher = this.cryptText(this.text, this.alphabetCipher, this.alphabet);
    },
    shiftArray(arr, num){
      return arr.slice(num).concat(arr.slice(0, num));
    },
    cryptText(text, alphabet,   alphabetCipher){
      const numArr =[];
      const textArr = text.split('');

      //проверка есть ли в ориг.алфавите такая буква
      textArr.forEach(el=>{
        if(alphabet.indexOf(el) !== -1){
          numArr.push(alphabet.indexOf(el))
        } else {
          numArr.push(el)
        }
      });
      console.log(numArr);
      //заменяем на букву из криптованного алфавита
      return numArr.map(n=>{
        let arr = [];

        //иначе если это символ, оставить как есть
        if  ( Number.isInteger(n) ){
          arr = alphabetCipher[n]
        } else {
          arr = n;
        }
        return  arr;
      }).join('');
    }
  }
}
</script>

<style>
  body {
    background-color: coral;
    font-family: 'Hachi Maru Pop', cursive;
  }
</style>
