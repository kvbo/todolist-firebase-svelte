<script>

export let label;
 function sayHi(){
    console.log('Hi mum')
 }

export let disabled = false;

</script>

<style>
    button {
        background-color: black;
        color: white;
        padding: 0.8rem 2rem;
        border-radius: 10px;
        font-weight: 700;
        font-size: 1rem;
    }
    button:disabled {
        opacity: 0.05;
    }
</style>

<button on:click disabled={disabled}>{label}</button>