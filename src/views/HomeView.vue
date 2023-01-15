<style scoped>
@import url('https://fonts.googleapis.com/css?family=Mukta');

body {
    font-family: 'Mukta', sans-serif;
    height: 100vh;
    min-height: 550px;
    background-color: black !important;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    position: relative;
    overflow-y: hidden;
}

a {
    text-decoration: none;
    color: #444444;
}

.login-reg-panel {
    position: relative;
    top: 50%;
    transform: translateY(-50%);
    text-align: center;
    width: 800px;
    margin: auto;
    height: 400px;
    background-color: rgba(236, 48, 20, 0.9);
}

.white-panel {
    background-color: rgba(255, 255, 255, 1);
    height: 500px;
    position: absolute;
    top: -50px;
    width: 50%;
    right: calc(50% - 300px);
    z-index: 0;
    box-shadow: 0 0 15px 9px #00000096;
}

.login-reg-panel input[type="radio"] {
    position: relative;
    display: none;
}

.login-reg-panel {
    color: #B8B8B8;
}

.login-reg-panel #label-register {
    border: 1px solid #9E9E9E;
    padding: 5px 5px;
    width: 150px;
    display: block;
    text-align: center;
    border-radius: 10px;
    cursor: pointer;
    font-weight: 600;
    font-size: 18px;
    margin: 15px 0;
}

.login-info-box {
    width: 30%;
    padding: 10px 50px;
    top: 20%;
    left: 0;
    position: absolute;
    text-align: left;
    margin: 15px 0;
}

.img {
  max-width: 100%;
  margin: 80px 50px;
}

</style>


<template>
    <div class="login-reg-panel">
        <div class="login-info-box">
            <h2>FirstChat</h2>
            <router-link to="/login">
              <label id="label-register" for="log-reg-show">Se connecter</label>
              <input type="radio" id="log-reg-show">
            </router-link>
            <router-link to="/register">
              <label id="label-register" for="log-reg-show">S'enregistrer</label>
              <input type="radio" id="log-reg-show">
            </router-link>
        </div>
    <div class="white-panel">
        <div class="img">
            <img src="../assets/mochi.png">
        </div>
    </div>
    </div>
</template>
