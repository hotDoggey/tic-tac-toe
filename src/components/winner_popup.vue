<template>
    <div class="grey-overlay">
        <div class="popup">
            <h3 class="hide popup-title">Placeholder Title</h3>
            <div class="main-content">
                <p>{{ popupText }}</p>
            </div>
            <button class="play-again btn" @click="this.$emit(`playAgainClicked`)">Play Again</button>
        </div>
    </div>
</template>

<script>
export default {
    name: "winner_popup",
    props: ["togglePopup", "popupText"],
    // props: {
    //     showWinnerPopup: Boolean,
    // },
    mounted() {},
    methods: {},
};
</script>

<style scoped>
.grey-overlay {
    position: fixed;
    top: 0;
    left: 0;
    background-color: rgba(62, 62, 62, 0.5);
    height: 100vh;
    width: 100vw;
}

.popup {
    position: relative;
    height: 20vh;
    width: 40vw;
    margin: calc(50vh - 35vh / 2) 0 0 calc(50vw - 35vw / 2);
    background-color: aliceblue;
    box-shadow: 7px 7px 60px rgb(0, 0, 0);
    border-radius: 3px;
}
.hide {
    opacity: 0;
}
.main-content {
    position: absolute;
    bottom: 50%;
    left: 50%;
    transform: translate(-50%, 25%);
    font-weight: 700;
    font-size: 1.5em;
}
.btn {
    background-color: #364c62;
    color: aliceblue;
    position: absolute;
    height: 30px;
    bottom: 28px;
    right: 30px;
    box-shadow: 0 5px #29394a;
    border-radius: 11px;
    border: 1px;
    font-weight: 700;
    font-size: 0.9em;
    outline: 0;
}
</style>
