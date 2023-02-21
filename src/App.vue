<template>
    <div id="website-background-image">
        <div id="main-content">
            <page_header />

            <tic_tac :timer="timer" :currentlyResettingAll="currentlyResettingAll" @winnerFound="presentWinner" @finishedResetting="toggleResettingAll" :togglePopup="togglePopup" />
            <!-- msg="passing some message as a prop into the component" -->
            <winner_popup ref="something" @playAgainClicked="playAgain" v-show="showWinnerPopup" :popupText="popupText" :togglePopup="togglePopup" />
        </div>
    </div>
</template>

<!-- ############## SCRIPT ############ -->
<script>
import tic_tac from "./components/tic_tac.vue";
import page_header from "./components/page_header.vue";
import winner_popup from "./components/winner_popup.vue";

export default {
    setup() {
        return {};
    },

    name: "App",
    components: {
        tic_tac,
        page_header,
        winner_popup,
    },
    data() {
        return {
            showWinnerPopup: 0,
            popupText: ``,
            currentlyResettingAll: false,
            timer: 10,
            tempMsg: `start timer`,
            interval: "",
        };
    },
    mounted() {},

    methods: {
        toggleResettingAll: function () {
            this.currentlyResettingAll = !this.currentlyResettingAll;
        },
        playAgain: function () {
            console.log(`restting all and getting ready to play again`);
            this.popupText = ``;
            this.togglePopup();
            this.toggleResettingAll();
        },
        // once the event "winnerFound" is emited from the tic_tac component, this function is run and it will take some variables passed in and will declare the winner
        presentWinner: function (winningSymbol, winningCombo) {
            console.log(`Winner is: ${winningSymbol}, Three box combo was achieved from: ${winningCombo}`);
            this.popupText = `${winningSymbol.toUpperCase()} is the winner!`;
            this.togglePopup();
        },
        togglePopup: function () {
            this.showWinnerPopup = !this.showWinnerPopup;
        },
    },
};
</script>

<!-- ############## STYLES ############ -->
<style>
body {
    margin: 0;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
#website-background-image {
    width: 100vw;
    min-height: 100vh;
    background-image: url("./assets/temps-crosses.png");
    background-attachment: fixed; /* means scrolling only moves content not backgroudn with it */
}
#main-content {
    width: 80%;
    min-width: 1050px;
    margin: auto;
    min-height: 100vh;
    background-color: rgb(230, 228, 228);
}
</style>
