<template>
  <div>
      <input 
      v-model="newTodo" @keyup.enter="addTodo"
      type="text" class="todo-input" 
      placeholder="What needs to be done">

      <transition-group name="fade" enter-active-class="animated fadeInUp"
        leave-active-class="animated fadeOutDown">
            <todo-item v-for="(todo, index) in todosFiltered" 
            :key="todo.id" 
            :todo="todo" 
            :index="index" 
            :checkAll="!anyRemaining"   
            /> 
      </transition-group>

      <div class="extra-container">
          <todo-check-all ></todo-check-all>
          <todo-items-remaining ></todo-items-remaining>
      </div>
      
      <div class="extra-container">
          <todo-filtered></todo-filtered>
          <div>
            <transition name="fade">
                <todo-clear-completed :showClearCompletedButton="showClearCompletedButton"></todo-clear-completed>
            </transition>
          </div>
          
      </div>


  </div>
</template>

<script>
import TodoItem from './TodoItem'
import TodoItemsRemaining from './TodoItemsRemaining'
import TodoCheckAll from './TodoCheckAll'
import TodoFiltered from './TodoFiltered'
import TodoClearCompleted from './TodoClearCompleted'

export default {
  name: 'todo-list',
  components: {
      TodoItem,
      TodoItemsRemaining,
      TodoCheckAll,
      TodoFiltered,
      TodoClearCompleted
  },
  data() {
      return {
          newTodo: '',
          idForTodo:3,
          beforeEditCache: '',
         
      }
  },
  computed:{
      remaining(){
          return this.$store.getters.remaining
      },
      anyRemaining() {
          return this.$store.getters.anyRemaining
      },
      todosFiltered() {
          return this.$store.getters.todosFiltered
      },
      showClearCompletedButton(){
          return this.$store.getters.showClearCompletedButton
      }
      
  },
  methods: {
      addTodo(){
        if(this.newTodo.trim().length == 0) {
            return
        }
        this.$store.dispatch('addTodo',{
            id: this.idForTodo,
            title: this.newTodo,
        })
        this.newTodo = " ";
        this.idForTodo++;
      },
  }
}
</script>


<style lang="scss">
    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css");
    .todo-input {
        width: 100%;
        padding:10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
    &:focus{
        outline:0;
        }
    }
    .todo-item{
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .remove-item{
        cursor: pointer;
        margin-left: 14px;
        &:hover{
            color: black
        }
    }
    .todo-item-left{
        display: flex;
        align-items: center;
    }
    .todo-item-label{
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
        
    }
    .todo-item-edit{
        font-size: 24px;
        color:#2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        &:focus{
            outline: none;
        }
    }
    .completed{
        text-decoration: line-through;
        color: grey;
    }
    .extra-container{
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
        transition: 0.2s;
    }
    button{
        font-size: 14px;
        background-color: white;
        padding:10px;
        
        appearance: none; 
        border: 0px solid #ccc;
        background-color: rgba(241, 241, 241, 0.548);
    
        border-radius: 45px;
        // box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
        transition: all 0.3s ease 0s;
        &:hover{
            background-color: #2EE59D;
            box-shadow: 0px 15px 20px rgba(46, 229, 157, 0.4);
            color: #fff;
        }
        &:focus{
            outline: none
        }
    }
    .clearButton {
        background-color: #2EE59D;
        color: #fff;
        cursor: pointer;
    }
    .active{
        background-color: #2EE59D;
        color: #fff;
    }

    .fade-enter-active, .fade-leave-active{
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to{
        opacity: 0;
    }
</style>
