<template>
  <v-card
    max-width="375"
    class="mx-auto border"
    dark
  >
    <v-row>
        <v-col class="container">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-calculator
            </v-icon>
            <h2>Calculadora
            </h2>
        </v-col>
    </v-row>
    <v-row>
        <v-col class="item result">
            <div v-if="result">
                <small>{{ labelCalculator }}</small>
                <p>{{ result | labelResult }}</p>
            </div>
            <div v-else>
                {{ labelCalculator }}
            </div>
        </v-col>
    </v-row>
    <v-row>
        <v-col class="item" v-on:click="actionHistoric('porcento')">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-percent-outline
            </v-icon>
        </v-col>
        <v-col class="item" v-on:click="actionHistoric('limparUltimo')">CE</v-col>
        <v-col v-on:click="actionHistoric('dividir')">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-division
            </v-icon>
        </v-col>
         <v-col v-on:click="clearAll()">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-close-box-multiple-outline
            </v-icon>
        </v-col>
    </v-row>
    <v-row>
        <v-col
            class="item"
            v-on:click="number(7)"
            >
            7
        </v-col>
        <v-col class="item" v-on:click="number(8)">8</v-col>
        <v-col class="item" v-on:click="number(9)">9</v-col>
        <v-col v-on:click="actionHistoric('multiplicar')">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-close
            </v-icon>
        </v-col>
    </v-row>
    <v-row>
        <v-col class="item" v-on:click="number(4)">
            4
        </v-col>
        <v-col class="item" v-on:click="number(5)">5</v-col>
        <v-col class="item" v-on:click="number(6)">6</v-col>
        <v-col v-on:click="actionHistoric('diminuir')">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-minus
            </v-icon>
        </v-col>
    </v-row>
    <v-row>
        <v-col class="item" v-on:click="number(1)">
            1
        </v-col>
        <v-col class="item" v-on:click="number(2)">2</v-col>
        <v-col class="item" v-on:click="number(3)">3</v-col>
        <v-col v-on:click="actionHistoric('somar')">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-plus
            </v-icon>
        </v-col>
    </v-row>
    <v-row>
        <v-col class="item" v-on:click="convert()">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-plus-minus-variant
            </v-icon>
        </v-col>
        <v-col class="item" v-on:click="number(0)">0</v-col>
        <v-col class="item" v-on:click="actionHistoric('pontuar')">,</v-col>
        <v-col v-on:click="equal()">
            <v-icon
                large
                color="green darken-2"
                class="item"
                >
            mdi-equal
            </v-icon>
        </v-col>
    </v-row>
  </v-card>
</template>

<script>
export default {
    filter: {
        labelResult(val) {
            return val.toLocaleString("pt-BR");
        }
    },
    data() {
        return {
            numbers: '',
            historic: [],
            result: '',
        }
    },
    computed: {
        labelCalculator() {
            let strResult = this.numbers;
            return strResult
                .replaceAll("/","÷")
                .replaceAll(".",",")
                .replaceAll("*","×");
        }
    },
    methods: {
        convert() {
            if(this.result){
                if(this.result >= 0){
                    this.result = `-${this.result}`;
                } else {
                    this.result = Math.abs(this.result).toString();
                }
            } else {
                if(this.numbers >= 0){
                    this.numbers = `-${this.numbers}`;
                } else {
                    this.numbers = Math.abs(this.numbers).toString();
                }
            }
        },
        clear(useResult = false) {
            if(this.result) {
                this.numbers = useResult ? this.result.toString() : '';
                this.result = '';
            }
        },
        clearAll() {
            this.numbers = '';
            this.result = '';
        },
        equal() {
            if(this.numbers) {
                this.result = eval(this.numbers);
                this.numbers = this.numbers.concat(" =");
            }
        },
        number(val) {
            this.clear();
            this.numbers = this.numbers.concat(val);
        },
        actionHistoric(action){
            this.clear(true);
            let ultimo = this.historic[this.historic.length -1];
            let value = "";
            if(action === "limparUltimo"){
                this.numbers.pop();
            }
            switch (action) {
                case 'multiplicar':
                    value = ' * ';
                    break;
                case 'diminuir':
                    value = ' - ';
                    break;
                case 'somar':
                    value = ' + ';
                    break;
                case 'dividir':
                    value = ' / ';
                    break;
                case 'porcento':
                    value = ' % ';
                    break;
                case 'pontuar':
                    if(!ultimo.includes('.')) {
                        value = '.';
                    }
                    break;
                default:
                    break;
            }
            const last = this.numbers.trim().slice(-1);
            if(!isNaN(Number(last))) {
                this.historic.push(last);
                this.numbers = this.numbers.concat(value);
            }
        },
    },
}
</script>

<style>
    .container {
        height: 200px;
        display: flex;
        flex-direction: row;
        align-items: start;
    }
    h2 {
        margin: 0 2%;
    }
    .mx-auto {
        margin: 20px auto;
        padding: 2%;
        border: 1px solid black;
    }
    .item, i {
        font-size: 22px !important;
        cursor: pointer;
    }
    .item:hover {
        color: green;
    }
    .result {
        text-align: right;
        margin: 0 2%;
    }
</style>