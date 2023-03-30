<!-- This component contains the game elements, the logic for adding 
    a symbol, and logic for checking for a winner after a symbol is
    added. EMITTED "winnerFound"; "finishedResetting"
    -->

<template>
    <h1 style="margin-top: 50px">{{ `Player ${currentSymbol} is next.` }}</h1>
    <div @playAgain="resetAll" class="tic-tac-container">
        <div id="play_area" ref="play_area">
            <div
                class="play-box"
                value="neitherXnorO"
                v-for="box in gridRefs"
                @click="addSymbol"
                :ref="box"
            >
                <img
                    class="hidden x-image"
                    src="..\assets\x2.png"
                    width="69"
                    height="69"
                />
                <img
                    class="hidden o-image"
                    src="..\assets\o.png"
                    width="69"
                    height="69"
                />
                <div @winnerFound="somethingToo"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "tic_tac",
    // props: [`togglePopup`],
    props: {
        togglePopup: Function,
        currentlyResettingAll: Boolean,
        timer: Number,
    },
    data() {
        return {
            currentSymbol: `x`, // x always starts first
            gridRefs: ["b0", "b1", "b2", "b3", "b4", "b5", "b6", "b7", "b8"],
            debug: 1,
            winner: ``,
            checkRows: [
                [0, 1, 2, `r1`],
                [3, 4, 5, `r2`],
                [6, 7, 8, `r3`],
            ],
            checkColumns: [
                [0, 3, 6, `c1`],
                [1, 4, 7, `c2`],
                [2, 5, 8, `c3`],
            ],
            checkDiagonals: [
                [0, 4, 8, `dLtR`],
                [2, 4, 6, `dRtL`],
            ],
        };
    },
    mounted() {},
    watch: {
        currentlyResettingAll(newVal, oldVal) {
            if (newVal === true) {
                this.resetAll();
            }
        },
    },

    methods: {
        addSymbol: function (e) {
            if (!this.winner) {
                // find the clicked on box
                const clickedBox = e.target.closest(`div.play-box`);
                // Only if clicked on box is empty
                if (!(clickedBox.value == `x` || clickedBox.value == `o`)) {
                    // 1. add current symbol as a class
                    clickedBox.value = `` + this.currentSymbol;

                    // 2. Render symbols in boxes
                    this.renderXsAndOs();

                    // 3. check for a winner
                    this.check4winner();

                    // 4. toggle current symbol
                    this.currentSymbol = this.currentSymbol == `x` ? `o` : `x`;
                } else if (this.debug) {
                    console.log(
                        `there is already a symbol (${clickedBox.classList}) in this box, so not adding currentSymbol (${this.currentSymbol})`
                    );
                }
            }
        },
        renderXsAndOs: function () {
            // find all boxes
            let boxes = this.$refs[`play_area`].children;

            // add correct symbol to each one [is HTMLCollection, so ... convert to array to allow use of forEach]
            [...boxes].forEach((box) => {
                // if x, add x; if o add o
                if (box.value) {
                    box.querySelector(`.${box.value}-image`).classList.remove(
                        `hidden`
                    );
                }
            });
        },
        check4winner: function () {
            // check rows, columns & diagonals
            this.checkArray(this.checkRows);
            this.checkArray(this.checkColumns);
            this.checkArray(this.checkDiagonals);
            if (this.winner === ``) console.log(`no winner yet"`);
        },
        // function to check a given array
        checkArray: function (arr) {
            arr.forEach((innerArr) => {
                let one = this.$refs[`b${innerArr[0]}`][0].value;
                let two = this.$refs[`b${innerArr[1]}`][0].value;
                let three = this.$refs[`b${innerArr[2]}`][0].value;
                // check that all three have a value in them
                if (one && two && three) {
                    if (one === two && one === three) {
                        // only if no winner yet
                        if (!this.winner) {
                            this.winner =
                                this.currentSymbol + ` ` + innerArr[3];
                            let [winningSymbol, winningCombo] =
                                this.winner.split(` `);
                            // emit event that gets actioned upon in App.vue
                            this.$emit(
                                `winnerFound`,
                                winningSymbol,
                                winningCombo
                            );
                        }
                    }
                }
            });
        },
        // reset all is fired by a watcher and resets the game, then emits an event to let main app know reset is done.
        resetAll: function () {
            // reset winner message
            (this.winner = ``), (this.currentSymbol = `x`);
            // find all boxes + remove symbol from each one
            let boxes = this.$refs[`play_area`].children;
            [...boxes].forEach((box) => {
                box.value = undefined;
                box.classList.remove(`x`, `o`);
                box.querySelector(`.x-image`).classList.add(`hidden`);
                box.querySelector(`.o-image`).classList.add(`hidden`);
                this.$emit(`finishedResetting`);
            });
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#play_area {
    display: grid;
    gap: 3px;
    grid-template-columns: repeat(3, 1fr);
    margin: 5% 50%;
    transform: translate(-50%, 0%);
    height: 400px;
    width: 400px;
}

#play_area > div {
    /* get all divs within the conainer */
    padding-top: 30%; /* 1:1 Aspect Ratio */
    padding-bottom: 30%;
    background-color: #ffffff;
    border: 2pt solid green;
    position: relative;
    border-radius: 3%;
}
.play-box img {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.hidden {
    opacity: 0;
}
</style>

<!-- TODO
    on mouse enter, display feint png, mouse leave remove it
-->
