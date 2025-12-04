<template>
    <form @submit.prevent="onSubmit">
        <div style ="display:flex;gap:8px;margin-bottom:8px;flex-wrap:wrap">
            <input v-model="form.name" placeholder="Class name" />
            <input v-model="form.coach" placeholder="Coach" />
        </div>

        <div class="form-row" style="margin-bottom:8px">
            <input type="date" v-model="form.date" />
            <input type="time" v-model="form.time" />
            <input type="number" v-model.number="form.capacity" min="1" placeholder="Capacity" />
        </div>

        <div style="display:flex;gap:8px;align-items:center">
            <button class="btn btn-primary" type="submit">Add session</button>
            <div class="small-muted">{{ message }}</div>
        </div>
    </form>
</template>

<script>
export default {
    name:"AddClassForm",
    data(){
        return {
            form:{name:"",coach:"",date:"",time:"",capacity:10},
        }
    },
    methods:{
        validate(){
            if(!this.form.name.trim()) return "Name required"
            if(!this.form.coach.trim()) return "Coach required"
            if(!this.form.date) return "Date required"
            if(!this.form.time) return "Time required"
            if(!this.form.capacity || this.form.capacity <10) return "Capacity must be at least 10"
            const when = new Date(`${this.form.date}T${this.form.time}`)
            if(isNaN(when)) return "Invalid date/time"
            return ""
        },
        onSubmit(){
            const err = this.validate()
            if(err){
                this.message = err
                return
            }
            this.$emit("add-session", {...this.form})
            this.message = "Added!"
            this.form = {name:"",coach:"",date:"",time:"",capacity:"8"}
            setTimeout(()=> this.message = "", 1500)
        }
    }
}
</script>