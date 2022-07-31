<script>
    import { tasks } from '../store'
    import CloseIcon from '../assets/CloseIcon.svelte'

    export let task;
    export let id;
    export let done;
    export let index

</script>
<style>
.task {
    display: flex;
    place-content: center;
    gap: 1rem;
    border-radius: 0.25rem;
    padding: 0.25rem;
    transition: .3s ease-in-out;
}
.task:hover {
    background-color: #fcfcfc;
}
button {
    box-sizing: border-box;
    padding: 4px;
    display: flex;
    place-content: center !important;
    place-items: center;
    background-color: transparent;
    border: none;
    font-size: 0.4rem;
    margin: auto 0;
    color: #888;
}
.container {
  display: block;
  position: relative;
  margin: auto 0;
  cursor: pointer;
  font-size: 1rem;

  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.container input {
  opacity: 0;
  cursor: pointer;
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 24px;
  width: 24px;
  border-radius: 50%;
  background-color: #eee;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #888;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

</style>

<div class='task' id="task-{id}">
    <label class="container" >
        <input type="checkbox" bind:checked={done} on:change="{e => $tasks[index].done = done }" />
        <span class="checkmark"></span>
    </label>
    <p style="
        color: #888;
        font-weight: 600;
        font-size: 0.8rem;
        "
    >{task}</p>
    <button on:click style="margin-left: auto;margin-right: 0 margin-top: auto; margin-bottom: auto"><CloseIcon /></button>
</div>

