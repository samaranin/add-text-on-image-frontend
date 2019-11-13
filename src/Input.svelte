<script>
    import { onMount } from 'svelte';

    export let label = "";
    export let placeholder = "";
    export let value = "";
    export let disabled = false;

    onMount(async () => {value = placeholder});
</script>

<style>
    label, input {
        display: inline;
        padding: 10px;
    }

    input {
        float: right;
        width: 59%;
    }

    div {
        width: 100%;
        display: flex;
        justify-content: center;
        height: auto;
    }

    @media (max-width: 640px) {
        div {
            display: block;
        }

        input {
            float: right;
            width: 100%;
        }
    }

    label {
        width: 40%;
    }

</style>

<div>
    <label>{label}</label>
    {#if !disabled} 
        <input type="text" placeholder="{placeholder}" value="{placeholder}" bind:value={value}>
    {:else}
        <input type="text" placeholder="{placeholder}" value="{placeholder}" bind:value={value} disabled>
    {/if}
    
</div>