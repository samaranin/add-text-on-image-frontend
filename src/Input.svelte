<script>
    export let label = "";
    export let placeholder = "";
    export let value = "";
    export let disabled = false;
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

    label {
        width: 40%;
    }

</style>

<div>
    <label>{label}</label>
    {#if !disabled} 
        <input type="text" placeholder="{placeholder}" bind:value={value}>
    {:else}
        <input type="text" placeholder="{placeholder}" bind:value={value} disabled>
    {/if}
    
</div>