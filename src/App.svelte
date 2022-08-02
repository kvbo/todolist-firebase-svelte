<svelte:head>
  <title>Home</title>
</svelte:head>

<script>
import Todos from './Todos.svelte'

import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
import { firebaseConfig } from "./firebase"

import { setContext } from "svelte";
import { writable } from "svelte/store";

const todos = writable([])

setContext('todos', { todos: todos })

const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app)

</script>

<style global>
@tailwind utilities;
@tailwind components;
@tailwind base;

</style>



<Todos />
