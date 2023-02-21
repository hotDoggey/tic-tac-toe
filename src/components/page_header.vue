<template>
    <div class="top_ribon">
        <img alt="Toe Logo" class="logo" src="../assets/logo.jpg" />
        <h1 class="title">Welcome to Tic Tac Toes 👣</h1>
    </div>
</template>

<script></script>

<style scoped>
.top_ribon {
    display: flex;
    padding: 30pt 10pt 30pt 30pt;
    background: #2c3e50;
    color: whitesmoke;
}
.top_ribon > * {
    /* border: 15px solid orange;  <<< uncomment this to see effect of *star* */
    margin: 15px;
}
img.logo {
    height: 119px;
    width: 119px;
    border-radius: 20%;
}
.title {
    height: 80%;
    margin-top: auto;
    margin-bottom: auto;
}
</style>
