<script>    
import Button from './components/Button.svelte'
import Task from './components/Task.svelte'

import { v4 } from 'uuid'
import moment from 'moment'
import { initializeApp } from 'firebase/app'
import { firebaseConfig } from './firebase'
import { getFirestore, setDoc, updateDoc, doc, collection, Timestamp } from 'firebase/firestore';

export let todo = {
    title: '',
    created: '',
    tasks: []
} 

let value= ''

export let isEdit=false

const app = initializeApp(firebaseConfig)
const db = getFirestore(app)

const save = () => {
  if(isEdit){
    const _doc = doc(db, 'todos', todo.id)
    
    delete todo.id

    updateDoc(_doc, todo)
  }

  else {
    todo.created = Timestamp.now()
    setDoc(doc(db, "todos", v4()), todo);
  }
}


</script>

<main >
<div>
    <div class="font-bold">
        Title {todo.title}
    </div>
    <div>
        Created: {todo.created && moment(new Date(1000 * todo.created)).format('LLL')}
    </div>
    <div class="flex gap-4 my-4">
      <button on:click="{null }" class="todobtn"> <span class="material-icons">add</span> Add task </button>
      <button on:click="{null }" class="todobtn"> <span class="material-icons">clear</span> Clear list</button>

      <button on:click="{() => save() }" class="todobtn mr-0 ml-auto"> <span class="material-icons">save</span> Save</button>
      <button on:click="{null }" class="todobtn bg-red-500 text-white"><span class="material-icons">delete</span>Delete</button>
    </div>

</div>
<div>
<main>
  <div class="parent">
     <div class="fg">
      <div style="display:flex; flex-direction:column;margin-bottom: 1rem;">
      {#each todo.tasks as task, index (task)}
        <Task 
          task={task.task} 
          id={task.id} 
          done={task.done}
          index={index}
          todo={todo}
          on:click="{() => {
            todo.tasks[index] = null;
            todo.tasks = todo.tasks.filter( i => i != null )
          }}"
        />
      {/each}
      </div>
      <div style="        
        display:flex;
        flex-direction: column;
        gap: 1rem;
        margin-top: auto;
        margin-bottom: 0;
        ">
          <input 
            style='
              width: 100%; 
              padding: 1.3rem .5rem; 
              box-sizing: border-box;
              border: none;
              background-color: #f8f8f8 ;
              color: #777;
              border-radius: 0.25rem;
              font-weight: bold;
              ' 
            bind:value placeholder="add task" type='text' 
          />
          <div style="width: 100%; display: flex; justify-content: flex-end; gap: 1rem; align-items: center;">
            <p>
              Total tasks: {todo.tasks.length}
            </p>
            <p style="margin-left: 0; margin-right: auto;">
              Completed tasks: {todo.tasks.filter( i => i.done === true ).length || 0 }
            </p>
            <Button 
              label='ADD' 
              on:click={() =>{ todo.tasks = 
                [...todo.tasks, {id: v4(), task: value , done: false }]
                
                value = '' 
              }}
                  
              disabled={ !(value.length > 2)? true : false } 
            />
            <Button label='CLEAR'/>
          </div>
      </div>
    </div>
  </div>
</div>
</main> 

<style>
p{
  font-size: 0.8rem;
  font-weight: 600;
  color: #888
}
.parent {
  position: relative;
  
}
.todobtn {
  @apply flex place-items-center gap-2 text-sm p-2 rounded-md;
}

.fg {
  position: relative;
  background-color: white;
  box-shadow: 0 0.4rem .5rem 0.1rem rgba(147, 147, 147, 0.1);
  display: flex;
  flex-direction: column;
  min-height: 50vh;
  min-width: 600px; 
  padding: 2rem 1rem; 
  border-radius: 0.5rem;
  /* z-index: 10; */
}
</style> 