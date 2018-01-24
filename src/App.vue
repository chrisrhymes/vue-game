<template>
    <div id="app">
        <section class="section">
            <div class="columns is-multiline">
                <div class="column is-12">
                    <table class="table is-bordered is-fullwidth">
                        <tbody>
                        <tr class="has-text-centered">
                            <cell></cell>
                            <cell></cell>
                            <cell></cell>
                        </tr>
                        <tr>
                            <cell></cell>
                            <cell></cell>
                            <cell></cell>
                        </tr>
                        <tr>
                            <cell></cell>
                            <cell></cell>
                            <cell></cell>
                        </tr>
                        </tbody>
                    </table>
                    <div class="modal" :class="{'is-active' : winner != ''}">
                        <div class="modal-background" v-on:click="resetGame()"></div>
                        <div class="modal-content">
                            <div class="notification is-success">
                                <div class="content">
                                    <h1 class="title">Winner: {{ winner }}</h1>
                                </div>
                            </div>
                        </div>
                        <button class="modal-close is-large" aria-label="close" v-on:click="resetGame()"></button>
                    </div>
                </div>
                <div class="column is-12">
                    <p class="has-text-centered">Made by <a href="http://www.csrhymes.com">C.S. Rhymes</a></p>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
    import Cell from './Cell.vue'
    export default {
        name: 'app',
        components: {
            'cell': Cell
        },
        data() {
            return {
                counter: 0,
                currentPlayer: 'X',
                cells: {},
                cellValues: [],
                winner: ''
            }
        },
        created()
        {
            this.cells = this.$children;
        },
        methods:
        {
            updatePlayer()
            {
                 if(this.currentPlayer === 'O')
                 {
                     this.currentPlayer = 'X';
                 } else {
                     this.currentPlayer= 'O';
                 }
            },
            checkResult()
            {
                this.winner = '';
                this.cellValues = [];
                this.cells.forEach((cell) => {
                    this.cellValues.push(cell.value);
                });

                console.log(this.cellValues);

                let hasWon = this.winningResults(this.cellValues[0], this.cellValues[1], this.cellValues[2])
                    || this.winningResults(this.cellValues[3], this.cellValues[4], this.cellValues[5])
                    || this.winningResults(this.cellValues[6], this.cellValues[7], this.cellValues[8])
                    || this.winningResults(this.cellValues[0], this.cellValues[3], this.cellValues[6])
                    || this.winningResults(this.cellValues[1], this.cellValues[4], this.cellValues[7])
                    || this.winningResults(this.cellValues[2], this.cellValues[5], this.cellValues[8])
                    || this.winningResults(this.cellValues[0], this.cellValues[4], this.cellValues[8])
                    || this.winningResults(this.cellValues[6], this.cellValues[4], this.cellValues[2])
                    || this.stalemate();

                if(hasWon)
                {
                    this.winner = this.currentPlayer;


                } else if(hasWon === 'Stalemate')
                {
                    this.winner = 'Stalemate';
                }
            },
            winningResults(p1, p2, p3)
            {
                if(p1 === '-') return false;
                if(p1 !== p2) return false;
                if(p1 !== p3) return false;

                return true;
            },
            stalemate()
            {
                if(this.cellValues.includes('-'))
                {
                    return false;
                } else {
                    return 'Stalemate';
                }
            },
            resetGame()
            {
                this.winner = '';
                this.cells.forEach((cell) => {
                    cell.value = '-';
                });
                this.cellValues = [];
            }
        }
    }
</script>

<style lang="scss">
    @import '../node_modules/bulma/bulma.sass';

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
    }

</style>
