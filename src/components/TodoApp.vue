<template>
    <div class = "list-view"> 
        <div class = "container">
        <h1>Список дел</h1>
        <div class ="form-group">
          <label for="new-task-title">Что нужно сделать? </label>
          <input
            class = "form-control"
             :class="{
                'is-valid': canCreateTask,
                'is-invalid': isInvalidNewTitleInput
                }"
            type ="text"
            id="new-task-title"
            v-model="newTaskTitle"
            @keyup.enter="createTask"
          />
          <span>Символов осталось: {{ availableTaskTitle }} </span>
        </div>
        <todo-list :tasks="undoneTasks" type = "Новые"/>
        <todo-list :tasks="doneTasks" type = "Выполненные"/>
      </div>
    </div>
</template>

<script>
import TodoList from "./TodoList"


const MAX_TASK_TITLE_LENGTH = 100;

export default {

    components: {
        TodoList
    },

    data: ()=>({
        newTaskTitle: "",
        tasks: []
    }),

    computed:{

        yourTasksTitle(){
            if(this.undoneTasks.length == 0){
                return "Новых задач нет";
            }
            return "Ваши задачи";
        },

        undoneTasks () {
           return this.tasks.filter(task => !task.isDone);
        },
        
        doneTasks () {
            return this.tasks.filter(task => task.isDone);
        },

        isInvalidNewTitleInput(){
            return this.newTaskTitle.length > 0 && !this.canCreateTask

        },
        availableTaskTitle () {
            const availableLength = MAX_TASK_TITLE_LENGTH - this.newTaskTitle.length
            if (availableLength>=0){
                return availableLength
            }
            return 0;

        },
        canCreateTask () {
            if (this.newTaskTitle.length > 0 && this.availableTaskTitle > 0) {
                return true;
            }
            return false;
        }

    },

    mounted() {

        window.eventbus.$on("todo-item-change", this.changeTask);
    },

    methods: {
        createTask() {
            if(!this.canCreateTask) return;
            this.tasks.push({
                isDone: false,
                title: this.newTaskTitle
            })
            this.newTaskTitle="";
        },
        changeTask(_task) {

            if (_task.isDone){
                this._delTask(_task);
                
                }
            if (!_task.isDone){
                _task.isDone= true;
                }
        },
        _delTask(_task){
            const index = this.tasks.indexOf(_task)
                if(index != -1) {
                    this.tasks.splice(index,1)
                }
        }
    }

}
</script>

<style>
 .list-view {
     text-align: left;
 }

</style>
