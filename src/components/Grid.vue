<template>
    <div>
        <div class="gameStatus" :class="gameStatusColor">
          {{ gameStatusMessage }}
        </div>
        <table class="grid">
          <tr>
            <cell name="1"></cell>
            <cell name="2"></cell>
            <cell name="3"></cell>
          </tr>
          <tr>
            <cell name="4"></cell>
            <cell name="5"></cell>
            <cell name="6"></cell>
          </tr>
          <tr>
            <cell name="7"></cell>
            <cell name="8"></cell>
            <cell name="9"></cell>
          </tr>
        </table>
    </div>
</template>

<script>
    import Cell from './Cell.vue'

    export default {
        components: { Cell },

        data () {
            return {
                activePlayer: 'O',
                gameStatus: 'turn',
                gameStatusMessage: `O's turn`,
                gameStatusColor: 'statusTurn',
                moves: 0,
                cells: {
                    1: '', 2: '', 3: '',
                    4: '', 5: '', 6: '',
                    7: '', 8: '', 9: ''
                },
                winConditions: [
                    [1, 2, 3], [4, 5, 6], [7, 8, 9], // rows
                    [1, 4, 7], [2, 5, 8], [3, 6, 9], // columns
                    [1, 5, 9], [3, 5, 7]             // diagonals
                ],
            }
        },

        computed: {
            nonActivePlayer () {
                if (this.activePlayer === 'O') {
                    return 'X'
                }

                return 'O'
            }
        },

        watch: {
            // watches for change in the value of gameStatus and changes the status
            // message and color accordingly
            gameStatus () {
                if (this.gameStatus === 'win') {
                    this.gameStatusColor = 'statusWin';
                    this.gameStatusMessage = `${this.activePlayer} Wins !`;
                    return
                } else if (this.gameStatus === 'draw') {
                    this.gameStatusColor = 'statusDraw';
                    this.gameStatusMessage = 'Draw !';
                    return
                }

                this.gameStatusMessage = `${this.activePlayer}'s turn`
            }
        },

        methods: {
            // changes the active player
            changePlayer () {
                this.activePlayer = this.nonActivePlayer
            },

            // returns the game status to the gameStatus property
            changeGameStatus () {
                if (this.checkForWin()) {
                     return this.gameIsWon()
                } else if (this.moves === 9) {
                    return 'draw'
                }

                return 'turn'
            },

            // checks for possible win conditions from the data
            checkForWin () {
                for (let i = 0; i < this.winConditions.length; i++) {
                    // gets a single condition wc from the whole array
                    let wc = this.winConditions[i];
                    let cells = this.cells;

                    // compares 3 cell values based on the cells in the condition
                    if (this.areEqual(cells[wc[0]], cells[wc[1]], cells[wc[2]])) {
                        return true
                    }
                }

                return false
            },

            // Check cell values
            areEqual () {
                var len = arguments.length;

                for (var i = 1; i < len; i++){
                    if (arguments[i] === '' || arguments[i] !== arguments[i-1])
                        return false;
                }
                return true;
            },

            gameIsWon () {

                Event.$emit('win', this.activePlayer);

                this.gameStatusMessage = `${this.activePlayer} Wins !`;

                Event.$emit('freeze');

                return 'win'
            }
        },

        created () {

            Event.$on('strike', (cellNumber) => {
                    this.cells[cellNumber] = this.activePlayer;
                    this.moves++;
                    this.gameStatus = this.changeGameStatus();
                    this.changePlayer()
            });

            Event.$on('gridReset', () => {
                Object.assign(this.$data, this.$options.data())
            })

        }
    }
</script>

<style>
.grid {
  width: 100%;
  border-collapse: collapse;
}

.gameStatus {
    color: black;
    margin-bottom: 30px;
    padding: 15px;
    font-size: 1.4em;
    font-weight: bold;
}
</style>
