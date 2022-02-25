<template>
  <v-card
    max-width="375"
    class="mx-auto"
    dark
  >
    <v-container>
        <v-row>
            <v-col class="col-6 left">
                <small>14:15</small>
            </v-col>
            <v-col class="col-6 right">
                <v-icon
                    large
                    color="green darken-2"
                    class="item"
                    >
                mdi-network-strength-1
                </v-icon>
                <v-icon
                    large
                    color="green darken-2"
                    class="item"
                    >
                mdi-wifi
                </v-icon>
                <v-icon
                    large
                    color="green darken-2"
                    class="item"
                    >
                mdi-battery-70
                </v-icon>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="container-class">
                <h3><img align="center" alt="Stefany-Vue" height="30" width="40" src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Vue.js_Logo_2.svg/888px-Vue.js_Logo_2.svg.png"> Calculadora</h3>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="result">
                <div v-if="result">
                    <h3 style="margin: 0">{{ labelCalculator }}</h3>
                    <h1>{{ result | labelResult }}</h1>
                </div>
                <div v-else>
                    <small></small>
                    <h1>{{ labelCalculator }}</h1>
                </div>
            </v-col>
        </v-row>
        <div class="body-calcule">
            <v-row>
                <v-col class="item" v-on:click="actionHistoric('porcento')">
                    <v-icon
                        large
                        color="green darken-2"
                        >
                    mdi-percent-outline
                    </v-icon>
                </v-col>
                <v-col class="item" v-on:click="actionHistoric('limparUltimo')">CE</v-col>
                <v-col v-on:click="actionHistoric('dividir')" class="item">
                    <v-icon
                        large
                        color="green darken-2"
                        >
                    mdi-division
                    </v-icon>
                </v-col>
                <v-col v-on:click="clearAll()" class="item">
                    <v-icon
                        large
                        color="green darken-2"
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
                <v-col class="item" v-on:click="actionHistoric('multiplicar')">
                    <v-icon
                        large
                        color="green darken-2"
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
                <v-col v-on:click="actionHistoric('diminuir')" class="item">
                    <v-icon
                        large
                        color="green darken-2"
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
                <v-col v-on:click="actionHistoric('somar')" class="item">
                    <v-icon
                        large
                        color="green darken-2"
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
                        >
                    mdi-plus-minus-variant
                    </v-icon>
                </v-col>
                <v-col class="item" v-on:click="number(0)">0</v-col>
                <v-col class="item" v-on:click="actionHistoric('pontuar')">,</v-col>
                <v-col v-on:click="equal()" class="item equal">
                    <v-icon
                        large
                        color="green darken-2"
                        >
                    mdi-equal
                    </v-icon>
                </v-col>
            </v-row>
        </div>
        <hr>
    </v-container>
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
                this.result = eval(this.numbers) ? eval(this.numbers) : "0" ;
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
    .container-class {
        margin: 0 1px;
        height: 270px;
        display: flex;
        flex-direction: row;
        align-items: start;
    }

    h3 {
        font-family: 'Roboto', sans-serif;
        margin: 0 20px;
        font-weight: 100;
    }

    h1 {
        font-weight: 300;
    }

    .mx-auto {
        margin: 20px auto;
        border: 1px solid black;
        border-radius: 30px !important;
        -webkit-box-shadow: 9px 5px 30px 0px #000000;
        box-shadow: 9px 5px 30px 0px #000000 !important;
    }

    .item,
    i {
        font-size: 22px !important;
        cursor: pointer;
    }

    .item:hover {
        background-color: #1DC46B;
        border-radius: 10px;
        -webkit-box-shadow: 11px 13px 15px -10px #000000;
        box-shadow: 11px 13px 15px -10px #000000;
    }

    .equal {
        background-color: #1DC46B;
        border-radius: 10px;
        -webkit-box-shadow: 11px 13px 15px -10px #000000;
        box-shadow: 11px 13px 15px -10px #000000;
    }

    .result {
        text-align: right;
        margin: 0 2%;
        min-height: 80px;
    }

    hr {
        width: 147px;
        margin: 45px auto 5px;
        padding: 1px;
        background: white;
        border-radius: 20px;
    }

    .body-calcule {
        padding: 20px;
    }

    .right {
        display: flex;
        justify-content: right;
        display: -webkit-flex;
        -webkit-justify-content: right;
        padding: 20px 30px 0px;
    }

    .left {
        display: flex;
        justify-content: left;
        display: -webkit-flex;
        -webkit-justify-content: left;
        padding: 30px 30px 0px;
    }
</style>