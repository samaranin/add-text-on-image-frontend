<script>
    import { onMount } from 'svelte';

    export let labelLeft = "";
    export let labelRight = "";
    export let value = "";

    onMount(async () => {value = "left"});
</script>

<style>
    label {
        display: inline-flex;
        padding: 10px 0px;
        cursor: pointer;
        width: 49%;
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
        opacity: 0;
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

    div {
        display: inline-flex;
        width: 100%;
    }

    .position-right {
        float: right;
    }

    .radiobutton {
        position: relative;
        top: 0;
        left: 0;
        height: 25px;
        width: 25px;
        min-width: 25px;
        height: 25px;
        width: 25px;
        background-color: #ededed;
        border-radius: 50%;
        box-shadow: 1px 2px 1px rgba(0, 0, 0, 0.5);
    }

    .radiobutton:focus, .radiobutton:hover {
        outline: none;
        background-color: rgba(61, 153, 112, 0.9);
    }

    input[type=radio]:checked ~ .radiobutton {
        background-color: rgba(61, 153, 112, 0.9);;
    }   

    .radiobutton:after {
        content: "";
        position: absolute;
        display: none;
    }

    input:checked ~ .radiobutton:after {
        display: block;
    }

    label .radiobutton:after {
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

    .label {
        padding-top: 3px;
        padding-left: 10px;
        text-overflow: ellipsis;
        overflow: hidden;
    }
</style>

<div>
    <label for="postioin_left">
        <input type="radio" id="postioin_left" bind:group={value} value={"left"} checked />
        <span class="radiobutton"></span>
        <span class="label">{labelLeft}</span>
    </label>

    <label class="position-right" for="postioin_right">
        <input type="radio" class="postion-right" id="postioin_right" bind:group={value} value={"right"} />
        <span class="radiobutton"></span>
        <span class="label">{labelRight}</span>
    </label>
</div>