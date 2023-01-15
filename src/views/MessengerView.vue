<style scoped>

.messenger {
    width: 100vw;
    height: 100vh;
    display: flex;
}

.chat-list {
    background: black;
    height: 100%;
    width: 20%;
}

.board {
    background: black;
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    border: thick double white;
}

.chat {
    background: grey;
    flex: 1 0;
    width: 100%;
    display: flex;
    flex-direction: column;
}

.user-entry {
    border: solid 1px black;
    padding: 5px;
    text-align: center;
    margin: 25px 0;
    font-weight: 700;
    font-size: medium;
    text-align: left;
    margin-left: 25px;
}

.user-entry:hover {
    color: red;
}

.from-message {
    background: rgb(57, 4, 4);
    color: white;
    font-size: medium;
    padding: 5px;  
}

.to-message {
    background: black;
    font-size: medium;
    color: white;
    padding: 5px;
}

.scope-input input[type="submit"] {
    width: 10%;
    background: #444444;
    color: #f9f9f9;
    border: none;
    padding: 10px;
    text-transform: uppercase;
    border-radius: 2px;
    cursor: pointer;
    height: 50px;
}

.scope-input {
    background: white;
    border: solid 1px black;
    display: flex;
}

textarea {
    resize: none;
    width: 90%;
    height: 50px;
    border: none;
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
            <div class="scope-input">
                <textarea class="champ" v-model="message" placeholder="Votre message"></textarea>
                <input type="submit" @click="sendMessage" value="Envoyez">
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
        },

        update(){
            setTimeout(()=>{
            this.fetchUsers();
            this.fetchDialog(this.selectedUser);
            this.update();
        }, 1000);
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
        this.update();
    }
}

</script>