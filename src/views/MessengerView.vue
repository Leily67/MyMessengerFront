<style scoped>

.messenger {
    width: 100vw;
    height: 100vh;
    display: flex;
}

.chat-list {
    background: purple;
    height: 100%;
    width: 20%;
}

.board {
    background: yellow;
    height: 100%;
    width: 80%;
    display: flex;
    flex-direction: column;
}

.chat {
    background: orange;
    flex: 1 0;
    width: 100%;
    display: flex;
    flex-direction: column;
}

.input {
    background: cyan;
    width: 100%;
    height: 100px;
}

.user-entry {
    border: solid 1px black;
    padding: 5px;
    text-align: center;
}

.from-message {
    border: solid 1px black;
    display: inline-block;
    background: grey;
    
}

.to-message {
    border: solid 1px black;
    background: black
}


</style>

<template>
    <div class="messenger">
        <div class="chat-list">
            <div class="user-entry" v-for="user in users" :key="user.id" @click="selectUser(user.id)">
                {{ user.username }}
            </div>

        </div>
        <div class="board">
            <div class="chat">
                <div v-for="message in conversation" :class="message.to_user == selectedUser ? 'to-message' : 'from-message'" :key="message.id">{{ message.content }}</div>
                
            </div>
            <div class="input">
                <input type="text" v-model="message" placeholder="Votre message">
                <button type="submit" @click="sendMessage" class="btn btn-primary">Envoyez</button>
            </div>
        </div>
    </div>
</template>


<script lang="ts">

export default {
    methods: {
       async fetchUsers() {
            const headers = new Headers();
            headers.append("Content-Type", "application/json");
            headers.append("Authorization", `Bearer ${localStorage.token}`)
            const options = {
                method: 'GET',
                headers: headers
            };

            const response = await fetch ("https://mymenssenger-backend.osc-fr1.scalingo.io/users", options)
            const result = await response.json()
            this.users = result.data;
        },
        selectUser(id:number) {
            this.selectedUser = id;
            this.fetchDialog(id);
        },

        async fetchDialog(id:number) {
            const headers = new Headers();
            headers.append("Content-Type", "application/json");
            headers.append("Authorization", `Bearer ${localStorage.token}`)
            const options = {
                method: 'GET',
                headers: headers
            };

            const response = await fetch (`https://mymenssenger-backend.osc-fr1.scalingo.io/users/${id}/messages`, options)
            const result = await response.json()
            this.conversation = result.data;
        
        },
        sendMessage(){
            console.log(this.message);
            const myHeaders = new Headers();
            myHeaders.append("Authorization", `Bearer ${localStorage.token}`);
            myHeaders.append("Content-Type", "application/json");

            const raw = JSON.stringify({
                "content": this.message,
                "to_user": this.selectedUser
            });

            const requestOptions = {
                method: 'POST',
                headers: myHeaders,
                body: raw,
            };

            fetch("https://mymenssenger-backend.osc-fr1.scalingo.io/messages", requestOptions)
            .then(result => this.fetchDialog(this.selectedUser))
            .catch(error => console.log('error', error));

            this.message = "";
        }

    },
    data(){
        return {
            users: [] as any[],
            selectedUser: -1,
            conversation: [] as any[],
            message: ""
        }
    },
    mounted() {
        this.fetchUsers();
        setTimeout(()=>{
            this.fetchUsers();
            this.fetchDialog(this.selectedUser);
        }, 1000);
    }
}

</script>