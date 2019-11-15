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
        background: #ededed;
        box-shadow: 1px 2px 1px rgba(0, 0, 0, 0.5);
    }

    :focus, input:hover {
        outline: none;
        border: 1px solid rgba(61, 153, 112, 0.9);
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
        <span class="focus-border">
            <i></i>
        </span>
    {:else}
        <input type="text" placeholder="{placeholder}" value="{placeholder}" bind:value={value} disabled>
    {/if}
    
</div>