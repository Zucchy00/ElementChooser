<script lang="ts">
    import { GripHorizontal } from 'svelte-bootstrap-icons';
    import { ArrowsFullscreen } from "svelte-bootstrap-icons";
    import { XLg } from "svelte-bootstrap-icons";
    import Element from './Element.svelte';
	import { onMount } from 'svelte';

    export let X: number, Y: number, Br: number, Bl: number, Fl: number, Fr: number;

    export let Remove = () => {};
    export let Reload = () => {};
    export let Over = () => {};
    export let Left = () => {};
    export let Full = () => {};
    export let index: number;
    export let element: number = 0;
    export let overElement: number;
    let MainElement:HTMLDivElement;
    let showFull= false;

    function toggleFullscreen() {
        Full()
    }

    function closeElement() {
        Remove();
    }

    function Swap(index: number, newIndex: number) {
        let ElementsThing = JSON.parse(String(localStorage.getItem("usages")));
        let old = ElementsThing[index];
        ElementsThing[index] = ElementsThing[newIndex];
        ElementsThing[newIndex] = old;
        localStorage.setItem("usages", JSON.stringify(ElementsThing));
        Reload();
    }

    onMount(()=>{
        Update()
    })

    setInterval(()=>{Update()}, 100)

    function updateFromAdded() {
        let ElementsThing = JSON.parse(String(localStorage.getItem("usages")));
        element = ElementsThing[index]
    }

    function Update() {
        if(element !=0 && element != 1 && element != 3 && element != 4 && element != 7) showFull = true
        else showFull = false
    }
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div bind:this={MainElement}
    class="outline outline-2 w-1/6 aspect-square flex justify-center items-center relative m-5 mx-8 rounded-xl outline-black dark:outline-white overflow-hidden"
    draggable="false"
    on:dragstart={(event) => {
        if (event.dataTransfer) {
            event.dataTransfer.setData('text/plain', index.toString());
            event.dataTransfer.effectAllowed = 'move';
        }
    }}
    
    on:dragend={(event) => {
        event.preventDefault();
        if(overElement !== -1) Swap(index, overElement);
    }}
    on:dragover={(event) => {
        event.preventDefault();
        Over();
    }}
    on:dragleave={() => {
        Left();
    }}
>
    {#if showFull}
        <button class="outline dark:text-white dark:outline-white outline-black opacity-25 hover:opacity-100 absolute top-2 right-2 hover:outline-2 outline-0 active:outline-1 transition ease-in dark:ease-out duration-100 p-1.5 h-8 w-8 rounded-md flex items-center justify-center" on:click={() => toggleFullscreen()}>
            <ArrowsFullscreen class="w-5 h-5" />
        </button>
    {/if}

    
    <button class="outline text-red opacity-25 hover:opacity-100 absolute top-2 left-2 hover:outline-2 outline-0 active:outline-1 outline-red transition ease-in dark:ease-out duration-100 p-1.5 h-8 w-8 rounded-md flex items-center justify-center" on:click={() => closeElement()}>
        <XLg class="w-5 h-5" />
    </button>

    <Element {element} updateElementChooser={()=>{updateFromAdded()}} bind:X bind:Y bind:Br bind:Bl bind:Fl bind:Fr {index} ElementClass={""}/>

    <!-- {#if index > 4}
        <button on:click={() => { Swap(index, index - 5); }} class="outline dark:text-white dark:outline-white outline-black dark:outline-white opacity-25 hover:opacity-100 absolute top-2 left-1/2 hover:outline-2 outline-0 active:outline-1 transition ease-in dark:ease-out duration-100 h-8 w-8 rounded-md flex items-center justify-center transform -translate-x-1/2" on:click={() => toggleFullscreen()}>
            <ArrowUp class="w-5 h-5" />
        </button>
    {/if}

    {#if index < 10}
        <button on:click={() => { Swap(index, index + 5); }} class="outline dark:text-white dark:outline-white outline-black dark:outline-white opacity-25 hover:opacity-100 absolute bottom-2 left-1/2 hover:outline-2 outline-0 active:outline-1 transition ease-in dark:ease-out duration-100 h-8 w-8 rounded-md flex items-center justify-center transform -translate-x-1/2" on:click={() => toggleFullscreen()}>
            <ArrowDown class="w-5 h-5" />
        </button>
    {/if}

    {#if (index + 1) % 5 != 0}
        <button on:click={() => { Swap(index, index + 1); }} class="outline dark:text-white dark:outline-white outline-black dark:outline-white opacity-25 hover:opacity-100 absolute top-1/2 right-2 hover:outline-2 outline-0 active:outline-1 transition ease-in dark:ease-out duration-100 h-8 w-8 rounded-md flex items-center justify-center transform -translate-y-1/2" on:click={() => toggleFullscreen()}>
            <ArrowRight class="w-5 h-5" />
        </button>
    {/if}

    {#if index != 0 && index != 5 && index != 10}
        <button on:click={() => { Swap(index, index - 1); }} class="outline dark:text-white dark:outline-white outline-black dark:outline-white opacity-25 hover:opacity-100 absolute top-1/2 left-2 hover:outline-2 outline-0 active:outline-1 transition ease-in dark:ease-out duration-100 h-8 w-8 rounded-md flex items-center justify-center transform -translate-y-1/2" on:click={() => toggleFullscreen()}>
            <ArrowLeft class="w-5 h-5" />
        </button>
    {/if} -->

    <button 
    on:mousedown={()=>{MainElement.draggable = true}}
    on:mouseup={()=>{MainElement.draggable = false}}
    class="hover:cursor-grab active:cursor-grabbing outline dark:text-white dark:outline-white outline-black dark:outline-white opacity-25 hover:opacity-100 absolute bottom-2 left-1/2 hover:outline-2 outline-0 active:outline-1 transition ease-in dark:ease-out duration-100 rounded-md flex items-center justify-center transform -translate-x-1/2">
        <GripHorizontal class="w-8 h-8" />
        <GripHorizontal class="w-8 h-8" />
    </button>
</div>
