<template>
    <button 
        v-if="showClearCompletedButton"
        @click="clearCompleted" class="clearButton"
        >Clear Completed
    </button>
    
</template>

<script>

// import eventBus from '../eventBus'

export default {
    name: 'todo-clear-completed',
    computed:{
       showClearCompletedButton(){
          return this.$store.getters.showClearCompletedButton
        }
    },
    methods:{
        clearCompleted(){
            this.$store.dispatch('clearCompleted')
        }
    }
}
</script>

