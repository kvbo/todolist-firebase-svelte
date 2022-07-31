<script>    
import { initializeApp } from "firebase/app";
import { getFirestore, collection, query, getDocs } from "firebase/firestore";
import { firebaseConfig } from "./firebase"
import Button from './components/Button.svelte'
import { tasks } from './store'
import Task from './components/Task.svelte'
import { v4 } from 'uuid'
import { getContext } from "svelte";


const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

const q = query(collection(db, "todos"))

const { todos } = getContext('todos')

if(!$todos.length){
  let temp = []
  getDocs(q).then( snap => snap.forEach( doc => {
    $todos = [...$todos, { id: doc.id , ...doc.data()}]
  })).catch( error => console.log(error.message))
}
let value = ''
</script>

<style>
  .workspace{
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    max-height: 100vh;
    position: relative;
  }
  .sidebar{
    position: sticky;
    background-color: rgb(63, 63, 63);
    /* color: ; */
    max-width: 15rem;
    min-height: 100vh;
    box-sizing: border-box;
    padding: 0.5rem;
  }
</style>

<main class="workspace">
  <div class="sidebar">
    <h3>TODOS</h3>
    {#each $todos as todo (todo)}
      <div class="">{todo.title}</div>
    {/each}
  </div>
  <div>

  </div>
</main>

<!-- <main>
  <button on:click={() => console.log($todos)} />
  <div class="parent">
     <div class="fg">
      <div style="display:flex; flex-direction:column;margin-bottom: 1rem;">
      {#each $tasks as task, index (task)}
        <Task 
          task={task.task} 
          id={task.id} 
          done={task.done}
          index={index}
          on:click={() => {
            tasks.update( prev => {
              prev[index] = null;
              prev = prev.filter( i => i != null )
              return prev
            })
          }}
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
              Total tasks: {$tasks.length}
            </p>
            <p style="margin-left: 0; margin-right: auto;">
              Completed tasks: {$tasks.filter( i => i.done === true ).length || 0 }
            </p>
            <Button 
              label='ADD' 
              on:click={() =>{ $tasks = 
                [...$tasks, {id: v4(), task: value , done: false }]
                
                value = '' 
              }}
                  
              disabled={ !(value.length > 2)? true : false } 
            />
            <Button label='CLEAR' on:click={() => tasks.set([]) } />
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
.fg::after {
  position: absolute;
  width: 25rem;
  height: 100%;
  background-color: #888;
  border-radius: 1rem;
  z-index:  -1;
  content: " ";
  top: -5rem;
  left: -6rem;
  box-shadow: 0 0.4rem .5rem 0.1rem rgba(147, 147, 147, 0.1);
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
</style> -->