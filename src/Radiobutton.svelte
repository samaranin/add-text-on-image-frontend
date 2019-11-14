<script>
    import { onMount } from 'svelte';

    export let labelLeft = "";
    export let labelRight = "";
    export let value = "";

    onMount(async () => {value = "left"});
</script>

<style>
    label {
        display: inline;
        padding: 10px;
        cursor: pointer;
    }

    input[type=radio] {
        position: relative;
        appearance: none;
        font-size: inherit;
        width: 1em;
        margin: 0;
        color: inherit;
        outline: none;
        transition: 300ms ease-out;
        cursor: pointer;
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
</style>

<div>
    <p>
        <label for="postioin_left">
            <input type="radio" id="postioin_left" bind:group={value} value={"left"} checked />
            <span>{labelLeft}</span>
        </label>
    </p>

    <p>
        <label for="postioin_right">
            <input type="radio" id="postioin_right" bind:group={value} value={"right"} />
            <span>{labelRight}</span>
        </label>
    </p>
</div>