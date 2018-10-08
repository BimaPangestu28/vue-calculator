<template>
  <div class="calculator">
    <div class="display">{{ current || 0 }}</div>

    <div @click="clear">C</div>
    <div @click="sign">+/-</div>
    <div @click="percent">%</div>
    <div @click="operators('÷')" class="operator">÷</div>

    <div @click="number(7)">7</div>
    <div @click="number(8)">8</div>
    <div @click="number(9)">9</div>
    <div @click="operators('x')" class="operator">x</div>

    <div @click="number(4)">4</div>
    <div @click="number(5)">5</div>
    <div @click="number(6)">6</div>
    <div @click="operators('-')" class="operator">-</div>

    <div @click="number(1)">1</div>
    <div @click="number(2)">2</div>
    <div @click="number(3)">3</div>
    <div @click="operators('+')" class="operator">+</div>

    <div @click="number(0)" class="zero">0</div>
    <div @click="point">.</div>
    <div @click="results" class="operator">=</div>
  </div>
</template>

<script>
  export default {
    data: () => ({
      current: '',
      previous: '',
      now: '',
      operator: ''
    }),

    methods: {
      clear() {
        this.current  = ''
        this.previous = ''
        this.now      = ''
        this.operator = ''
      },

      sign() {
        if(this.operator) {
          if(this.now != '')
            this.now   = (toString(this.now).charAt() === '-') ? 
                         this.now.slice(1) : `-${this.now}`

          this.current = `${this.previous} ${this.operator} (${this.now})`
        } else {
          if(typeof this.current == 'number') {
            if(this.current != '')
            this.current = (this.current > 0) ? 
                           this.current - (2 * this.current) : this.current + (-2 * this.current)
          } else {
            if(this.current != '' && this.current != '0.')
            this.current = (this.current.charAt() === '-') ? 
                           this.current.slice(1) : `-${this.current}`
          }
        } 
      },

      percent() {
        if(this.operator)
          this.aritmatika(this.operator, this.previous, this.now, 'result')

        this.current  = this.current / 100

        this.now      = ''
        this.operator = ''
      },

      isFloat(number) {
        return Number(number) === number && number % 1 !== 0
      },

      point() {
        if(this.operator) {
          if(this.now)
            this.now = (this.now.indexOf(".") == '-1') ?
                       `${this.now}.` : this.now

          this.current = `${this.previous} ${this.operator} ${this.now}`
        } else {
          if(this.current) {
            if(this.isFloat(this.current))
              this.current = this.current
            else
              this.current = (this.current.indexOf(".") == '-1') ?
                       `${this.current}.` : this.current
          } else {
            this.current = '0.'
          }
        }
      },

      number(number) {
        this.current = (this.current == '0') ?
                       parseFloat(this.current) + parseFloat(number) : `${this.current}${number}`

        this.now     = (this.operator) ? `${this.now}${number}` : ''
      },

      operators(operator) {
        this.aritmatika(this.operator, this.previous, this.now, 'next')

        this.operator = operator
        this.current  = this.previous
        this.current  += ` ${this.operator} `
      },

      aritmatika(operator, previous, now, type) {
        switch(operator) {
          case '+':
            if(type == 'result') {
              this.current  = parseFloat(previous) + parseFloat(now)
            } else {
              this.previous = (this.operator) ? 
                              parseFloat(previous) + parseFloat(now) : this.current

              if(this.operator)
                this.current  = this.previous

              this.now = ''
            }
            break;

          case '-':
            if(type == 'result') {
              this.current  = parseFloat(previous) - parseFloat(now)
            } else {
              this.previous = (this.operator) ? 
                              parseFloat(previous) - parseFloat(now) : this.current

              if(this.operator)
                this.current  = this.previous

              this.now = ''
            }
            break;

          case 'x':
            if(type == 'result') {
              this.current  = parseFloat(previous) * parseFloat(now)
            } else {
              this.previous = (this.operator) ? 
                              parseFloat(previous) * parseFloat(now) : this.current

              if(this.operator)
                this.current  = this.previous

              this.now = ''
            }
            break;

          case '÷':
            if(type == 'result') {
              this.current  = parseFloat(previous) / parseFloat(now)
            } else {
              this.previous = (this.operator) ? 
                              parseFloat(previous) / parseFloat(now) : this.current

              if(this.operator)
                this.current  = this.previous

              this.now = ''
            }
            break;

          default:
            if(type == 'next')
              this.previous = this.current
            break;
        }
      },

      results() {
        this.aritmatika(this.operator, this.previous, this.now, 'result')
        this.operator = ''
        this.now      = ''
      }
    }
  }
</script>

<style scoped>
  .calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
    width: 300px;
    margin: 0 auto;
  }

  .calculator div {
    border: 1px solid #ededed;
    padding: 20px;
    font-size: 22px;
    cursor: pointer;
  }

  .display {
    grid-column: 1 / 5;
    background: violet;
    border: 1px solid violet !important;
    color: white;
    cursor: normal;
  }

  .zero {
    grid-column: 1 / 3;
  }

  .operator {
    background-color: orange;
    color: white;
    border: 1px solid orange !important;
  }
</style>
