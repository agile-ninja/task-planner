<template>
  <div id="task-planner">
      
    <h1>Ninja Task Planner</h1>
    

    <div class="planner-column first">
        <h2>To do ({{ todo_number }})</h2>
        <div class=list>
            <draggable v-model="todos" class="dragArea" :options="{group:'task'}">
              <li v-for="todo in todos">{{todo.fields.text}}</li>
            </draggable>

            
            <textarea v-on:keyup.enter="add_item(1, $event)" v-model="new_todo" placeholder="Add a task"></textarea>
        </div>
    </div>
    <div class="planner-column second">
        <h2>Wip ({{ wip_number }})</h2>
        <div class=list>
            <draggable v-model="wips" class="dragArea" :options="{group:'task'}">
              <li v-for="wip in wips">{{wip.fields.text}}</li>
            </draggable>
           
            <textarea v-on:keyup.enter="add_item(2, $event)" v-model="new_wip" placeholder="Add a task"></textarea>
       
        </div>
    </div>
    <div class="planner-column third">
        <h2>Done ({{ done_number }})</h2>
        <div class=list>
            <draggable v-model="dones" class="dragArea" :options="{group:'task'}">
              <li v-for="done in dones">{{done.fields.text}}</li>
            </draggable>
            
           
            <textarea class="left" v-on:keyup.enter="add_item(3, $event)" v-model="new_done" placeholder="Add a task"></textarea>
       </div>    
    </div>

</div>

</template>

<script>
import draggable from 'vuedraggable'
import axios from 'axios';

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
      errors: [
      ],
      todos: [
      ],
      wips: [  
      ],
      dones: [
      ],
      new_todo: '',
      new_wip: '',
      new_done: '',
      fields: { text: '', status: ''}
    }
  },
  // Fetches posts when the component is created.
  created() {
    //Fetch todos
    axios.get("https://api.airtable.com/v0/appPXztooplH2PBFw/Task?maxRecords=3&view=Grid%20view&filterByFormula=AND({status} = 'todo')",
    { headers: { Authorization: "Bearer key9wgomi43BEzb0T" } })
    .then(response => {
      // JSON responses are automatically parsed.
      this.todos = response.data.records
      this.todo_number = this.todos.length
      console.log(this.todos)
    })
    .catch(e => {
      console.log(e)
      this.errors.push(e)
    })
    //Fetch wips
    axios.get("https://api.airtable.com/v0/appPXztooplH2PBFw/Task?maxRecords=3&view=Grid%20view&filterByFormula=AND({status} = 'wip')",
    { headers: { Authorization: "Bearer key9wgomi43BEzb0T" } })
    .then(response => {
      // JSON responses are automatically parsed.
      this.wips = response.data.records
      this.wip_number = this.wips.length
      console.log(this.wips)
    })
    .catch(e => {
        console.log(e)
      this.errors.push(e)
    })
    //Fetch dones
    axios.get("https://api.airtable.com/v0/appPXztooplH2PBFw/Task?maxRecords=3&view=Grid%20view&filterByFormula=AND({status} = 'done')",
    { headers: { Authorization: "Bearer key9wgomi43BEzb0T" } })
    .then(response => {
      // JSON responses are automatically parsed.
      this.dones = response.data.records
      this.done_number = this.dones.length
      console.log(this.dones)
    })
    .catch(e => {
        console.log(e)
      this.errors.push(e)
    })
  },
  watch: {
     
      todos: function() {   
          this.todo_number = this.todos.length
      },
      wips: function() {   
          this.wip_number = this.wips.length
      },
      dones: function() {   
          this.done_number = this.dones.length
      },
  },
  computed: {
      todo_numbers: function(){
          console.log("aprendi");
          this.todo_number = this.todos.length
      },
      wip_numbers: function(){
          this.wip_number = this.wips.length
      },
      done_numbers: function(){
          this.done_number = this.dones.length
      },
  },
  methods: {
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
            
            var d = new Date();
            var n = d.toLocaleDateString(); 

            if (step == 1) {
                this.fields = { text: event.target.value, status: 'todo', creation_date: n }    
                this.todos.push( {fields: this.fields})
                this.new_todo = ''
            } else if (step == 2) {
                this.fields = { text: event.target.value, status: 'wip', creation_date: n }    
                this.wips.push( {fields: this.fields})
                this.new_wip = ''
            } else {
                this.fields = { text: event.target.value, status: 'wip', creation_date: n }  
                this.dones.push( {fields: this.fields})
                this.new_done = ''
            }
            
            //Fetch wips
            axios.post("https://api.airtable.com/v0/appPXztooplH2PBFw/Task?api_key=key9wgomi43BEzb0T",
            {
                "fields": this.fields
                
            })
            .then(response => {
                console.log("exito")
            })
            .catch(e => {
                console.log(e)
                this.errors.push(e)
            })              
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
