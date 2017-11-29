<template>
    <div id="app">
        <div class="row text-center">
            <h1>JOSH'S CALCULATOR</h1>
            <div class="main-body col-xs-10 col-xs-offset-1 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3 col-lg-4 col-lg-offset-4">
                <div class="row">
                    <div class="screen col-xs-10 col-xs-offset-1 col-sm-8 col-sm-offset-2 col-md-10 col-md-offset-1">
                        <div class="entry">
                            <h5>{{entry}}</h5>
                        </div>
                        <h6>History:</h6>
                        <div class="past-entries">
                            <template v-for="(item, index) in entries">
                                <li class="left">{{entries[index]}}</li>
                                <li class="right">= {{answers[index]}} </li>
                            </template>
                        </div>
                    </div>
                </div>
                <br>
                <div class="row" v-for="(row, i) in buttons">
                    <template v-for="val in buttons[i]">
                        <button
                        type="button"
                        class="btn btn-lg"
                        :class="computeColor(val)"
                        @click="updateDisplay(val)">
                        {{val}}</button>
                    </template>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                buttons: [
                    ["AC","del","(",")"],
                    ["7","8","9","/"],
                    ["4","5","6","*"],
                    ["1","2","3","-"],
                    ["0",".","=","+"]
                ],
                entry: '',
                entries: [],
                answers: []
            }
        },
        methods: {
            updateDisplay(button) {
                if(this.entry =="=ERROR"){
                    this.clearDisplay();
                }
                switch (button) {
                    case "AC":
                        this.clearDisplay();
                        this.clearHistory();
                        break;
                    case "=":
                        let ans = 0;
                        try {
                            ans = eval(this.entry);
                            this.entries.unshift(this.entry);
                            this.answers.unshift(ans);
                            this.clearDisplay();
                            this.entry = this.answers[0];
                        }
                        catch(err) {
                            this.entry = "=ERROR";
                        }
                        break;
                    case "del":
                        this.deleteOne();
                        break;
                    default:
                        this.entry += button;
                        break;
                }
            },
            clearDisplay() {
                this.entry = '';
            },
            clearHistory() {
                this.entries = [];
                this.answers = [];
            },
            deleteOne() {
                if(typeof(this.entry) != "string") {
                    this.entry = String(this.entry);
                }
                this.entry = this.entry.slice(0,-1);
            },
            computeColor(val) {
                let buttonColor = '';
                switch (val) {
                    case "AC":
                    case "del":
                        buttonColor = 'btn-danger';
                        break;
                    case "(":
                    case ")":
                    case "/":
                    case "*":
                    case "-":
                    case "+":
                        buttonColor = 'btn-warning'
                        break;
                    case "=":
                        buttonColor = 'btn-primary';
                        break;
                    default:
                        buttonColor = 'btn-default'
                }
                return buttonColor;
            }
        }
    }
</script>

<style>
    body {
        background: #3CA55C;  /* fallback for old browsers */
        background: -webkit-linear-gradient(to right, #B5AC49, #3CA55C);  /* Chrome 10-25, Safari 5.1-6 */
        background: linear-gradient(to right, #B5AC49, #3CA55C); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
        font-family: 'Roboto Slab', serif;
    }

    .main-body {
        background-color: #357a75;
        border-radius: 25px;
        padding-bottom: 25px;
    }

    .screen {
        background: #848484;
        border-radius: 20px;
        margin-top: 25px;
        height: 225px;
        font-size: 1.5em;
        overflow: hidden;
        padding: 0;
        box-shadow: 1px 3px black;
    }
    li {
        list-style: none;
    }
    .entry {
        min-height: 75px;
        border: 1px solid black;
        border-radius: 15px;
        margin:10px;
    }
    .past-entries{
        margin: 15px;
    }

    .left {
        text-align:left;
    }

    .right {
        text-align:right;
        font-size: 1.5em;
    }

    h5 {
        font-size: 2em;
    }

    button {
        margin: 10px;
        height: 25%;
        min-width: 50px;
        width: 10%;
    }
</style>
