<template>
    <div>
        <div v-if="!sessions.length" class="empty">No sessions scheduled - add one using the form.</div>

        <div class="session-list" v-else>
            <Session-card
                v-for="s in ordered"
                :key="s.id"
                :session="s"
                @delete="onDelete"
                />
        </div>
</div>
</template>

<script>
import SessionCard from './SessionCard.vue'
export default {
    name: 'SessionList',
    props:{sessions:{type:Array,required:true}},
    components:{SessionCard},
    computed:{
        ordered(){
            return this.sessions.slice().sort((a,b)=> new Date(`${a.date}T${a.time}`) - new Date(`${n.date}T${b.time}`))
        }
    },
    methods:{
        onDelete(id){ this.$emit("delete-session", id) }
    }
}
</script>