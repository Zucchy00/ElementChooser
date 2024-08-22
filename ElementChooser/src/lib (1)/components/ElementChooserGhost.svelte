<script lang="ts">
    import { onMount} from 'svelte';
    import { ConeStriped, PlusLg } from "svelte-bootstrap-icons";

    export let Add = () => {};
    export let Over = () => {};
    export let Left = () => {};
    export let index: number;

    let element: HTMLDivElement;
    let usedElement: boolean;
    let outline: HTMLElement;

    function addElement() {
        Add();
    }

    function MouseOver() {
        Over()
    }

    function MouseLeft() {
        Left()
    }

    onMount(() => {
        usedElement = false;
        if (element) {
            new PlusLg({
                target: element
            });
            usedElement = false;
        }
    });
</script>
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div 
bind:this={outline} 
on:dragover={(event) => {
    event.preventDefault();
    Over();
}}
on:dragleave={() => {
    Left();
}}
class="outline-dashed outline-2 transition duration-500 w-1/6 opacity-30 aspect-square flex justify-center items-center relative m-5 mx-8 rounded-xl outline-black dark:outline-white">
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <button
        on:click={addElement}
        on:mouseenter={() => {
            outline.className = "outline-dashed outline-2 transition duration-500 w-1/6 opacity-80 aspect-square flex justify-center items-center relative m-5 mx-8 rounded-xl outline-black dark:outline-white";
        }}
        on:mouseleave={() => {
            outline.className = "outline-dashed outline-2 transition duration-500 w-1/6 opacity-30 aspect-square flex justify-center items-center relative m-5 mx-8 rounded-xl outline-black dark:outline-white";
        }}
    >
        <PlusLg class="dark:text-white w-8 h-8 hover:cursor-pointer active:w-6 active:h-6 hover:w-10 hover:h-10 transition ease-in dark:ease-out duration-100"/>
        {#if usedElement}
            <div bind:this={element}></div>
        {/if}
    </button>
</div>
