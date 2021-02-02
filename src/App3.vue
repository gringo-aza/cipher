<template>
  <div id="app" class="container mx-auto mt-4">

    <div class="flex flex-wrap mb-3">

      <div class="w-full md:w-2/4 ml-auto px-2">
        <label for="textarea1">
          <h1 class="text-3xl md:text-4xl
              font-bold text-white mb-2">Шифр Цезаря</h1>
        </label>
        <textarea
                id="textarea1"
                class="md:text-xl sm:text-base resize-none text-gray-600 border
                 rounded focus:outline-none
                 focus:shadow-outline w-full p-3 shadow"
                size="lg"
                rows="7"
                placeholder="Напишите текст чтобы зашифровать его"
                v-model="text"
                @keyup.46="text = textCipher = ''"></textarea>
      </div>

      <div class="w-full md:w-2/4 mr-auto px-2">
        <label>
          <h1 class="text-3xl md:text-4xl
              font-bold text-white mb-2">Результат</h1>
        </label>
        <textarea
                class="md:text-xl sm:text-base resize-none text-gray-800
          border rounded focus:outline-none focus:shadow-outline w-full p-3 shadow"
                size="lg"
                rows="7"
                disabled
                v-model="textCipher"></textarea>
      </div>

    </div>

    <div class="flex flex-wrap">

      <div class="w-full md:w-1/6 ml-auto px-2">
        <label class="block uppercase tracking-wide
             text-gray-800 text-xs font-bold mb-2" for="grid-state">
          Сдвиг <strong>{{ selected }}</strong>
        </label>

        <div class="relative w-40">
          <select class="block appearance-none w-full shadow
          bg-gray-200 border border-gray-200 text-gray-700 py-3 px-4 pr-8
          rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
                  id="grid-state"
                  v-model="selected">
            <option value="">Выбрать</option>
            <option v-for="(val, i) in 33" :key="i">{{ val }}</option>
          </select>
        </div>
      </div>

      <div class="w-full md:w-5/6 ml-auto px-2 mt-6 md:pl-12">
        <button class="btn btn-blue mr-4"
                @click="encrypt">
          <svg class="w-8 h-8 mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd"
                  d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z"
                  clip-rule="evenodd" />
          </svg>
          <span>Зашифровать</span>
        </button>

        <button class="btn btn-green"
                @click="decrypt">
          <svg class="w-8 h-8 mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
            <path d="M10 2a5 5 0 00-5 5v2a2 2 0 00-2 2v5a2 2 0 002 2h10a2 2 0 002-2v-5a2 2 0 00-2-2H7V7a3 3 0 015.905-.75 1 1 0 001.937-.5A5.002 5.002 0 0010 2z" />
          </svg>
          <span>Расшифровать</span>
        </button>
      </div>

    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        selected: "",
        text: '',
        textCipher: '',
        alphabet: 'АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ',
        alphabetCipher: '',
      }
    },
    methods: {
      encrypt() {
        this.alphabetCipher = this.alphabet
        this.alphabetCipher = this.shiftArray( this.alphabetCipher.split(''), this.selected ).join('')
        this.textCipher = this.cryptText( this.text, this.alphabet, this.alphabetCipher )
      },
      decrypt() {
        this.alphabetCipher = this.alphabet
        this.alphabetCipher = this.shiftArray( this.alphabetCipher.split(''), this.selected ).join('')
        this.textCipher = this.cryptText( this.text, this.alphabetCipher, this.alphabet )
      },
      shiftArray(arr, num) {
        return arr.slice(num).concat( arr.slice(0, num) )
      },
      cryptText(text, alphabet, alphabetCipher) {
        const numArr = []
        const textArr = text.split('')
        // добавляем в алфавит и нижний его регистр
        alphabet = alphabet + alphabet.toLowerCase()
        alphabetCipher = alphabetCipher + alphabetCipher.toLowerCase()
        // проверка есть ли в ориг.алфавите такая буква
        textArr.forEach(el => {
          if ( alphabet.indexOf(el) !== -1 ) {
            numArr.push( alphabet.indexOf(el) )
          } else {
            numArr.push(el)
          }
        })
        // заменяем на букву из криптованного алфавита
        return numArr.map(n => {
          let arr = []
          // иначе если это символ, оставить как есть
          if ( Number.isInteger(n) ) {
            arr = alphabetCipher[n]
          } else {
            arr = n
          }
          return arr
        }).join('')
      }
    }
  }
</script>

<style>
</style>