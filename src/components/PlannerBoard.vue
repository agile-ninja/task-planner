<template>
  <div id="task-planner">
      
    <h1>Ninja Task Planner</h1>
    

    <div class="planner-column first">
        <h2>To do ({{ todo_number }})</h2>
        <div class=list>
            <draggable v-model="todos" class="dragArea" :options="{group:'task'}">
              <li v-for="todo in todos">{{todo.text}}</li>
            </draggable>

            
            <textarea v-on:keyup.enter="add_item(1, $event)" v-model="new_todo" placeholder="Add a task"></textarea>
        </div>
    </div>
    <div class="planner-column second">
        <h2>Wip ({{ wip_number }})</h2>
        <div class=list>
            <draggable v-model="wips" class="dragArea" :options="{group:'task'}">
              <li v-for="wip in wips">{{wip.text}}</li>
            </draggable>
           
            <textarea v-on:keyup.enter="add_item(2, $event)" v-model="new_wip" placeholder="Add a task"></textarea>
       
        </div>
    </div>
    <div class="planner-column third">
        <h2>Done ({{ done_number }})</h2>
        <div class=list>
            <draggable v-model="dones" class="dragArea" :options="{group:'task'}">
              <li v-for="done in dones">{{done.text}}</li>
            </draggable>
            
           
            <textarea class="left" v-on:keyup.enter="add_item(3, $event)" v-model="new_done" placeholder="Add a task"></textarea>
       </div>    
    </div>

</div>

</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'PlannerBoard',
  components: {
    draggable
  },
  data () {
    return {
      todo_number: 0,
      wip_number: 0,
      done_number: 0,
      todos: [
      ],
      wips: [  
      ],
      dones: [
      ],
      new_todo: '',
      new_wip: '',
      new_done: ''
    }
  },methods: {
      add: function(step) {
            if (step == 1) {
                this.todo_number++
            } else if (step == 2) {
                this.wip_number++
            } else {
                this.done_number++
            }
        },
        add_item: function(step, event) {
            if (step == 1) {
                this.todo_number++
                this.todos.push({ text: event.target.value })
                this.new_todo = ''
            } else if (step == 2) {
                this.wip_number++
                this.wips.push({ text: event.target.value })
                this.new_wip = ''
            } else {
                this.done_number++
                this.dones.push({ text: event.target.value })
                this.new_done = ''
            }                
        },
        todo_move: function(id) {
             this.wips.push(this.todos[id])
             this.todos.splice(id, 1)               
        },
        wip_move: function(id, direction) {
             if (direction == 0) {
                 this.todos.push(this.wips[id])
             } else {
                 this.dones.push(this.wips[id])            
             }
             this.wips.splice(id, 1)   
        },
        done_move: function(id) {
            this.wips.push(this.dones[id]) 
            this.dones.splice(id, 1)   
        }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#task-planner {
    width: 80%;
    margin: 0 auto;
}
.planner-column {
    width: 33%; 
    float: left; 
    font-size: 14px;
}
.planner-column .list {
    min-height: 400px;
    padding: 20px;
    margin: 10px;
}    
.planner-column.first .list {
    background-color: #1F324A;
}
.planner-column.second .list {
    background-color: #0190AA;
}
.planner-column.third .list {
    background-color: #73B5BC;
}
.planner-column .list li {
    background-color: #f7f9f9;
    height: 50px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    list-style: none;
    margin-bottom: 15px;
    padding: 3%;
    position: relative;
}
.planner-column .list li:hover {
    cursor: pointer;
}   
.planner-column textarea {
    background-color: #f7f9f9;
    width: 94%;
    height: 50px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    list-style: none;
    margin-bottom: 15px;
    resize: none;
    font-size: 14px;
    color: #6b7588;
    padding: 3%;
}

.planner-column button {
    position: absolute;
    right: 0px;
    bottom: 0px;
}
.planner-column button.left {
    position: absolute;
    right: 40px;
    bottom: 0px;
}
.dragArea {
    min-height: 10px;
}
</style>
