<template>
  <v-container>
    <text>
      <h1 style="margin-left:25%">Calculadora do CRS</h1>
    </text>
    <v-container style="width:400px; margin-right: 450px; margin-top:70px">
      <v-form style="">
        <v-row>
          <v-text-field
          v-model="visor"
          placeholder="0"
          label="Calculadora"
          outlined
          disabled
          key-code
          v-if="!forget"
          />
          <v-text-field
          v-model="resultado"
          placeholder="0"
          label="Calculadora"
          outlined
          disabled
          v-if="forget"
          />
          <v-btn
          outlined
          @click="cls()"
          color="blue"
          style="margin-left:20px">
            Limpar
          </v-btn>
        </v-row>
        <v-row>
          <v-col>
            <v-btn
            @click="pegarDigitos(7)">
              7
            </v-btn>  
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos(8)">
              8
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos(9)">
              9
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos('-')">
              -
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-btn
            @click="pegarDigitos(4)">
              4
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos(5)">
              5
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos(6)">
              6
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos('+')">
              +
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-btn
            @click="pegarDigitos(1)">
              1
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos(2)">
              2
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos(3)">
              3
            </v-btn>

          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos('/')">
              /
            </v-btn>  
          </v-col>
        </v-row>
        <v-row>
          <v-col
          cols="6">
            <v-btn
            @click="pegarDigitos(0)"
            style="width:165px">
                0
            </v-btn>
          </v-col>
          <v-col>
            <v-btn
            @click="pegarDigitos('=')">
              =
            </v-btn>
          </v-col>
           <v-col>
            <v-btn
            @click="pegarDigitos('*')">
              *
            </v-btn>  
          </v-col>
        </v-row>
      </v-form>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'Calculadora',
  data() { 
   return {
      forget:null,
      numeros: [],
      visor: null,
      resultado: 0,
      algarismos: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
      i: 0

   }
  },
  methods: {
    pegarDigitos (digito) {
      try {
        if(!this.algarismos.includes(digito)){
          if(digito === '='){ 
            this.carcule();
          }
          let aux = this.numeros.splice(this.i).join('')
          this.i++;
          this.numeros.push(aux)
          this.i++
        }
        this.numeros.push(digito)
        this.visor = this.numeros.join('')
      } catch (error) {
        this.$toast.error(error)
      }
    },
    carcule () {
      let numeros = this.numeros
      this.resultado = numeros[0];
      numeros.forEach((value, i) =>{
        
        if(i % 2 != 0){
          switch(value) {
            case '+': {
              this.resultado += numeros[i+1]
              break;
            }
            case '-': {
               this.resultado -= numeros[i+1]
              break;
            }
            case '*': {
              this.resultado *= numeros[i+1]
              break;
            }
            case '/': {
              this.resultado =  this.resultado / numeros[i+1]
              break;
            }
          }
        }
      })
      this.forget = true
      this.visor = this.resultado
    },
    cls () {
      this.numeros = [];
      this.visor = null
      this.resultado = null;
      this.forget = null
      this.i = 0;
    }
  }
}
</script>
<style>
</style>