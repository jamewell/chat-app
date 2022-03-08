<template>
    <div class="relative h-10 m-1">
        <div class="grid grid-cols-6" style="border-top: 1px solid #e6e6e6;">
            <input type="text" class="col-span-5 outline-none p-1"
                v-model="message"
                @keyup.enter="sendMessage()"
                placeholder="What would you like to say..."
                >
            <button 
                @click="sendMessage()"
                class="place-self-end bg-gray-500 hover:bg-blue-700 p-1 mt-1 rounded text-white">
                Send
            </button>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import Input from '../../Jetstream/Input.vue'
export default {
    components: {
        Input
    },
    props: ['room'],
    data: function() {
        return {
            message: ''
        }
    },

    methods: {
        sendMessage() {
            console.log('message', this.message);
            if(this.message == ' '){
                return;
            }

            axios.post('/chat/room/' + this.room.id + '/message', {
                message: this.message
            })
            .then((result) => {
                if(result.status == 201){
                    this.message = '';
                    this.$emit('messagesent');
                }
            }).catch((err) => {
                console.log(err);
            });
        }
    }
}
</script>