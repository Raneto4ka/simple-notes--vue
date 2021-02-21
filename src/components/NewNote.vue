<template>
    <div class="new-note">
        <label for="">Title</label>
        <input v-model="note.title" type="text">
        <div class="input-group">
            <label>
                <input v-model="note.priority" value="Standart" id="priority1" type="radio">
                Priority Standart
            </label>
            <label>
                <input v-model="note.priority" value="Hight" id="priority2" type="radio">
                Priority Hight
            </label>
            <label>
                <input v-model="note.priority" value="Extra Hight" id="priority3" type="radio">
                Priority Extra Hight
            </label>
        </div>

        <label for="">Description</label>
        <textarea v-model="note.description"></textarea>
        <button class="btn btnPrimary mt-4" @click="addNote">New note</button>
    </div>    
</template>

<script>
export default {
    props: {
        note: {
            type: Object,
            required: true,            
        },          
    },
    methods: {
        addNote() {
            this.$emit('addNote', this.note)
        },
    },
}
</script>

<style lang="scss">
    .new-note {
        text-align: center;
    }
    .mt-4 {
        margin-top: 16px;
    }
    .input-group {
        display: flex;
        align-items: center;
        justify-content: space-between;
        input, label {
            display: inline-flex;
            align-items: baseline;
            width: auto;
            margin-right: 16px;
        }
    }

</style>