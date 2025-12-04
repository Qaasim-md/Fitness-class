<template>
    <div class="session-card">
        <div class ="session-info">
            <div><strong>{{ session.name }}</strong> - {{ session.coach }}</div>
            <div class="session-meta">{{ prettyDate }} at {{ session.time }} • capacity {{ session.capacity }}</div>
        </div>
        <div style="display:flex;gap:8px;a;ign-items:center">
            <button class="del" @click="emitDelete">Delete</button>
        </div>
    </div>
</template>

<script>
export default {
    name:"SessionCard",
    props:{session:{type:Object,required:true}},
    computed:{
        prettyDate(){
            return new Date(this.session.date).toLocaleDateString()
        }
    },
    methods:{
        emitDelete(){ this.$emit("delete", this.session.id) }
    }
}
</script>