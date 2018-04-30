<template>
    <div class="container">
        <section class="hero is-primary">
            <div class="hero-body typewriter">
                <h1 class="title">
                    tic tac toe, bro?
                </h1>
            </div>
        </section>
        <div id="app">
            <div class="card">
                <div class="card-content">
                    <transition name="slide-fade">
                        <grid v-if="show"></grid>
                    </transition>
                    <button class="button is-warning restart" @click="restart">Restart</button>
                </div>
                <footer class="card-footer scoreBoard">
                    <div class="card-footer-item">
                        O - {{ wins.O }}
                    </div>
                    <div class="card-footer-item">
                        X - {{ wins.X }}
                    </div>
                </footer>
            </div>
        </div>
    </div>
</template>

<script>
import Grid from './components/Grid.vue'

export default {

    components: {
        Grid
    },

    name: 'app',

    data () {

        return {

            show: true,

            matches: 0,

            wins: {
              O: 0,
              X: 0
            }

        }

    },

    methods: {

        restart() {

            this.toggleFade();

            Event.$emit('clearCell');
            Event.$emit('gridReset');
            this.match++;

            setTimeout(() => {
                this.toggleFade()
            }, 800);

        },

        toggleFade() {
            return this.show = !this.show;
        }

    }
}
</script>

<style>
    .slide-fade-enter-active {
        transition: all .3s ease;
    }
    .slide-fade-leave-active {
        transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }
    .slide-fade-enter, .slide-fade-leave-to {
        transform: translateX(10px);
        opacity: 0;
    }
    .typewriter h1 {
        overflow: hidden;
        white-space: nowrap;
        margin: 0 auto;
        letter-spacing: .15em;
        animation:
            typing 5s steps(50, end),
            blink-caret .75s step-end infinite;
    }
    @keyframes typing {
        from { width: 0 }
        to { width: 100% }
    }
    body {
      color: #fff;
      font-family: 'Dosis', Helvetica, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      margin: 0px;
    }
    #app {
        margin: 0 auto;
        max-width: 350px;
        color: #34495e;
    }
    .card {
        box-shadow: none;
    }
    h1 {
      text-transform: uppercase;
      font-weight: bold;
      font-size: 3em;
    }
    .restart {
        font-size: 1.4em;
        font-weight: bold;
        margin-top: 30px;
        width: 100%;
    }
    .restart:hover {
        cursor: pointer;
    }
    .scoreBoard {
        color: white;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;
        width: 100%;
        height: 15px;
        background-color: #16a085;
        padding: 20px;
        font-weight: bolder;
    }
</style>
