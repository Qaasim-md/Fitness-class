<template>
    <div class="container">
        <div class="header">
            <div>
                <h1>Flexzone - Class Scheduler</h1>
                <div class="small-muted">Manage sessions locally - ready to extend to a booking backend.</div>
            </div>
            <div class="card" style="padding:10px">
                <div class="small-muted">Total sessions</div>
                <div class="count">{{ totalSessions }}</div>
        </div>
    </div>

    <div class="grid">
        <div>
            <div class="card">
                <h2 style="margin-top:0">Scheduled Sessions</h2>
                <session-list
                    :sessions="sessions"
                    @delete-session="deleteSession"
                    />
            </div>

            <div style="height:18px"></div>

            <div class="card">
                <h3 style="margin=top:0">Next Up</h3>
                <div v-if="nextSession" class="session-card">
                    <div class ="session-info">
                        <div><strong>{{ nextSession.name}}</strong> - {{ nextSession.coach }}</div>
                        <div class="session-meta">{{ formatData(nextSession.date) }} at {{ nextSession.time }} • capacity {{ nextSession.capacity }}</div>
                    </div>
                    <button class="del" @click="deleteSession(nextSession.id)">Cancel</button>
                    </div>
                    <div v-else class="empty">No upcoming sessions</div>
                </div>
            </div>

            <div>
                <div class="card">
                    <h2 style="margin-top:0">Add Class</h2>
                    <AddClassForm @add-session="addSession" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import AddClassForm from './components/AddClassForm.vue'
import SessionList from './components/SessionList.vue'

export default {
    name: 'App',
    components:{AddClassForm, SessionList},
    data(){
        return {sessions: []}
    },
    computed: {
        totalSessions(){
            return this.sessions.length
        },
        nextSession(){
            if(!this.sessions.length) return null
            const now = new Date()
            const upcoming = this.sessions
            .map(s=>({...s, _ts: new Date('${s.date}T${s.time}')}))
            .filter(s=>s._ts >= now)
            .sort((a,b)=>a._ts - b._ts)
            return upcoming.length ? upcoming[0] : null
        }
    },
    methods:{
        addSession(session){
            session.id = Date.now().toString(36) + Math.random().toString(36).slice(2,6)
            this.sessions.push(session)
            this.save()
        },
        deleteSession(id){
            this.sessions = this.sessions.filter(s=>s.id !== id)
            this.save()
        },
        save(){
            localStorage.setItem('fz_sessions', JSON,stringify(this.sessions))
        },
        load(){
            try{
                const raw = localStorage.getItem('fz_sessions')
                if(raw) this.sessions = JSON.parse(raw)
            }catch(e){ console.warn('load failed', e) }
        },
        formatDate(d){
            const dt = new Date(d)
            return dt.toLocaleDateString()
        }
    },
    mounted(){
        this.load()
    }
}
</script>