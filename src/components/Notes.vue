<template>
    <div class="notes">
        <div class="note" :class="[{ full: !grid }, priorityClass(index)]" v-for="(note, index) in notes" :key="index">
            <div class="note-header" :class="{ full: !grid }">
                <p @dblclick="editTitle(index)" :contenteditable="note.contenteditable" class="title" ref="p">{{ note.title }}</p>
                <p style="cursor:pointer;" @click="removeNote(index)">x</p>
            </div>
            <div class="note-body">
                <p>{{ note.description }}</p>
                <p>{{ note.date }}</p>
            </div>
        </div>
    </div>   
</template>

<script>
export default {
    props: {
        notes: {
            type: Array,
            required: true,
        },
        grid: {
            type: Boolean,
            required: true,
        },
    },
    created() {
        document.addEventListener('click', (e) => {
            if(e.target.getAttribute('contenteditable') == "true") {
                return;
            }
            else {
                for(let i = 0; i < this.notes.length; i++) {
                    
                    for(let key in this.notes[i]) {
                        if (key === 'contenteditable' & this.notes[i][key] === true) {
                                //console.log(this.notes[i]); 
                                //console.log(this.$refs.p[i].innerText);
                                this.updateTitle(this.$refs.p[i].innerText, i);
                        }
                    }
                }
            }
        });                         
    },
    computed: {
    },
    methods: {
        priorityClass(index) {
            let priorityName = this.notes[index].priority;
            if(priorityName === "Hight" ) return "hight"
            else if(priorityName === "Extra Hight") return "extra-hight"
            else return ""
        },
        editTitle(index) {  
            this.notes[index].contenteditable = true;     
        },
        removeNote(index) {
            this.$emit('remove', index)
        },
        updateTitle(text, index) {
            //console.log(text);
            //console.log(index);
            this.$emit('updateTitle', {
                title: text,
                index: index
            })           
        },
    }
}
</script>

<style lang="scss">
    .notes {
        display: flex;
        align-items: stretch;
        justify-content: space-between;
        flex-wrap: wrap;
        padding: 40px 0;
    }
    .note {
        width: 48%;
        padding: 18px 20px;
        margin-bottom: 20px;
        background-color: #fff;
        transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
        box-shadow: 0 30px 30px rgba(0,0,0,0.2);
        &:hover {
         box-shadow: 0 30px 30px rgba(0,0,0,0.4);   
         transform: translate(0, -6px);
         transition-delay: 0s !important;        
        }
        &.full {
            width: 100%;  
            text-align: center;
        }
        &.hight {
            background-color: rgb(224, 255, 53);
        }
        &.extra-hight {
            background-color: rgb(224, 255, 53);   
            border: 2px solid #494ce8;         
        }
    }
    .note-header { 
        display: flex;
        align-items: center;
        justify-content: space-between;
        h1 {font-size: 32px;}
        p {
            color: #402caf;
            font-size: 22px;
            &[contenteditable=true] {
                border: 1px solid #000;
                padding: 0 20px;
            }
        }
        svg {
            color: #999;
            margin-right: 12px;
            cursor: pointer;
            &:hover {
                color: #000;
            }
            &:last-child {
               margin-right: 0; 
            }
            &.active {
                color: #402caf;
            }
        }
        &.full {
            justify-content: center;
            p {
                margin-right: 16px;
                &:last-child {
                    margin-right: 0;
                }

            }
        }
    }
    .note-body {
        p {
            margin: 20px 0;
        }
        span {
            font-size: 14px;
            color: #999;
        }
    }

</style>
