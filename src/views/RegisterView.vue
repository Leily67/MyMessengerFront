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
    top: 50%;
    transform: translateY(-50%);
    text-align: center;
    width: 800px;
    right: 0;
    left: -50px;
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
    padding: 0 50px;
    top: 20%;
    left: 0;
    position: absolute;
    text-align: left;
    margin: 15px 0;
}

.register-show {
    z-index: 1;
    color: black;
    text-align: left;
    padding:50px;
}

.register-show input[type="text"], .register-show input[type="password"], .register-show input[type="mail"] {
    width: 100%;
    display: block;
    margin: 11px 0;
    padding: 15px;
    border: 1px solid #b5b5b5;
    outline: none;
}

.register-show input[type="submit"] {
    max-width: 150px;
    width: 100%;
    background: #444444;
    color: #f9f9f9;
    border: none;
    padding: 10px;
    text-transform: uppercase;
    border-radius: 2px;
    float: right;
    cursor: pointer;
}


</style>


<template>
    <div class="login-reg-panel">
        <div class="login-info-box">
            <h2> Vous avez un compte ?</h2>
            <router-link to="/">
                <label id="label-register" for="log-reg-show">Home</label>
                <input type="radio" name="active-log-panel" id="log-reg-show">
            </router-link>
            <router-link to="/login">
                <label id="label-register" for="log-reg-show">Se connecter</label>
                <input type="radio" name="active-log-panel" id="log-reg-show">
            </router-link>
        </div>
    <div class="white-panel">
        <div class="register-show">
            <h2>S'enregister</h2>
            <input type="text" v-model="username" placeholder="Pseudo">
            <input type="text" v-model="lastname" placeholder="Nom">
            <input type="text" v-model="firstname" placeholder="Prénom">
            <input type="password" v-model="password" placeholder="Mot de passe">
            <input type="mail" v-model="mail" placeholder="Email">
            <input type="submit" @click="register" value="Valider">
        </div>
    </div>
    </div>
</template>

<script lang ="ts">

export default {
    methods: {
        register(){
            console.log(this.username);
            const myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");
            const raw = JSON.stringify({
                "username": this.username,
                "firstname": this.firstname,
                "lastname": this.lastname,
                "password": this.password,
                "mail": this.mail
            });
            

            const requestOptions = {
                method: 'POST',
                body: raw,
                headers : myHeaders
            };

            fetch(`${import.meta.env.VITE_BACKEND_URL}/register`, requestOptions)
                .then(response => {
                    this.$router.push('/login');
                })
                .catch(error => console.log('error', error));
        }
        
    },
    data(){
        return {
            username:"",
            firstname:"",
            lastname:"",
            password:"",
            mail:""
        }
    }
}
</script>