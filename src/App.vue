<template>
    <div id="app">
        <div class="container">
            <TheGasStation/>
        </div>
    </div>
</template>

<script>

    import TheGasStation from "./components/TheGasStation/TheGasStation";

    export default {
        name: 'app',
        components: {
            TheGasStation
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .container {
        position: relative;
        width: 460px;
        height: auto;
        margin: 0 auto;
    }
</style>
