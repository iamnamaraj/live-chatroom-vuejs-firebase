<template>
    <div class="chat-window">
        <div v-if="error" class="error">{{ error }}</div>
        <div v-if="documents" class="messages" ref="messages">
            <div v-for="doc in formatedDocuments" :key="doc.id" class="single">
                <span class="created-at">{{ doc.createdAt }}</span>
                <span class="name">{{ doc.name }}</span>
                <span class="message">{{ doc.message }}</span>
            </div>
        </div>
    </div>
</template>

<script>
import { computed, onUpdated, ref } from 'vue'
import getCollection from '../composables/getCollection'
import { formatDistanceToNow } from 'date-fns'
export default {
    setup(){
        const { error, documents } = getCollection('messages')
        const messages = ref(null)

        const formatedDocuments = computed (()=>{
            if(documents.value){
                return documents.value.map(doc => {
                    let time = formatDistanceToNow(doc.createdAt.toDate())
                    return {...doc, createdAt:time}

                } )
            }
        })

        onUpdated(()=>{
            messages.value.scrollTop = messages.value.scrollHeight;
        })
        return { error, documents,formatedDocuments, messages }
    }
}
</script>

<style>
    .chat-window{
        background: #fafafa;
        padding: 30px 20px;
    }
    .single{
        margin: 18px 0;
    }
    .created-at{
        display: block;
        margin-bottom: 4px;
        font-size: 12px;
        color: #999;
    }
    .name{
        margin-right: 10px;
        font-weight: bold;
    }
    .messages {
        max-height: 400px;
        overflow: auto;
    }
</style>