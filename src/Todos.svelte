<script>    
import { initializeApp } from "firebase/app";
import { getFirestore, collection, query, getDocs, orderBy, doc, deleteDoc } from "firebase/firestore";
import { firebaseConfig } from "./firebase"

import { getContext } from "svelte";
import Todo from "./Todo.svelte";
import App from "./App.svelte";


const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

const q = query(collection(db, "todos"), orderBy('created', 'desc'))

const { todos } = getContext('todos')


if(!$todos.length){
  let temp = []
  getDocs(q).then( snap => snap.forEach( doc => {
    $todos = [...$todos, { id: doc.id , ...doc.data()}]
  })).catch( error => console.log(error.message))
}

const delete_todo = (e, id) => {
  e.preventDefault()
  const doc_to_be_deleted = doc(collection(db, "todos"), id)
  deleteDoc(doc_to_be_deleted).then(_ => console.log(_)).catch(error => console.log(error))
}


let selected = {}

</script>

<style>
  .workspace{
    display: flex;
    flex-direction: rows;
    min-height: 100vh;
    max-height: 100vh;
    position: relative;
    overflow: hidden;
  }
  .sidebar{
    /* position: sticky; */
    max-width: 18rem;
    min-height: 100vh;
    box-sizing: border-box;
    flex: 1;
  }
  .main {
    flex: 1;
    overflow-y: auto;
    box-sizing: border-box;
  }
  .todo {
    border-radius: 4px;
    padding: 0.5rem 0.5rem;
    opacity: 0.8;
    cursor: pointer;
    width: full;
  }
  .todo:hover{
    background-color: #888;
  }
</style>

<main class="workspace">
  <div class="sidebar bg-gradient-to-b from-slate-700 to-slate-800 text-white select-none flex flex-col">
    <div class="min-h-[10vh] flex px-8 place-items-center border-b">
      <div class="">Todos</div>
      <button 
        on:click="{() => selected = {created: '', title: '', tasks: [] }}"
        class="mr-0 ml-auto flex place-items-center hover:transform hover:scale-110 transition-transform">
        <span class="material-icons">add</span>
      </button>
    </div>


    <div class="box-border px-2 py-2 overflow-y-auto text-sm border-b flex-1">
      {#each $todos as todo (todo)}
        <div class='flex place-items-center 
            opacity-75  
            border-box
            { selected.id == todo.id && "text-[#0f0] opacity-100 bg-white bg-opacity-10 "}
            hover:text-[#0f0]
            cursor-pointer 
            py-3 
            px-6 
            hover:opacity-100
            hover:bg-opacity-10
           hover:bg-white
           h-[3rem]
           group 
           rounded-md' on:click="{() => selected = todo }">
          <div class="flex-1">{todo.title}</div>
          <button 
            on:click="{e => delete_todo(e, todo.id)}"
            class="mr-0 ml-auto hidden group-hover:flex place-items-center hover:transform hover:scale-110 transition-transform">
          <span class="material-icons">
            delete
          </span>
        </button>
        </div>
      {/each}
    </div>
    <div class="min-h-[10vh] py-4 flex px-8 place-items-center opacity-70 font-semibold text-sm">
      Total: {$todos.length}
    </div>
  </div>

  <div class="main">
    <div class="min-h-[10vh] border-b shadow-sm bg-white"></div>
    <div class="px-4">
      {#if Object.entries(selected).length > 0 }
        {#if selected.id}
          <Todo todo={selected} isEdit={true}/>
        {:else }
          <Todo todo={selected} isEdit={false} />
        {/if}
      {:else }
        <div>Nothing to show</div>
      {/if}
    </div>   
  </div>

</main>

