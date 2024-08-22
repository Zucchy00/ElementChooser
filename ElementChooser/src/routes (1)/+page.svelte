<script lang="ts">
    import Clock from "$lib/components/Clock.svelte";
	import Element from "$lib/components/Element.svelte";
    import ElementChooser from "$lib/components/ElementChooser.svelte";
    import ElementChooserGhost from "$lib/components/ElementChooserGhost.svelte";
    import { onMount } from "svelte";
	import { ArrowsAngleContract } from "svelte-bootstrap-icons";

    let elements = 0;
    let ElementsThing: number[] = []
    let background: HTMLDivElement
    let title: HTMLDivElement
    let X: number = 0, Y: number = 0, Br: number = 0, Bl: number = 0, Fl: number = 0, Fr: number = 0
    let loaded: boolean = false
    let timeout: number;
    let overElement: number = -1
    let fullscreen:boolean = false
    let fullscreenID = -1

    // Define a function to handle the mousemove event
    function handleMouseMove() {
        if (elements === 0) {
            background.className = "absolute top-1/2 bg-black opacity-0 transition duration-1000 w-screen h-screen left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
            title.className = "absolute opacity-0 top-1/2 left-1/2 transform duration-1000 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
            clearTimeout(timeout)
            timeout = setTimeout(function () {
                if (elements === 0) {
                    background.className = "absolute top-1/2 bg-black opacity-60 transition duration-1000 w-screen h-screen left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
                    title.className = "absolute opacity-100 top-1/2 left-1/2 transform duration-1000 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
                }
            }, 3000);
        }
    }

    function addElement(index: number) {
        if (elements < 15) {
            elements += 1;
            ElementsThing = JSON.parse(String(localStorage.getItem("usages")));
            ElementsThing[index] = 0
            localStorage.setItem("usages", JSON.stringify(ElementsThing))
        }
    }

    function removeElement(index: number) {
        if (elements > 0) {
            elements -= 1;
            ElementsThing = JSON.parse(String(localStorage.getItem("usages")));
            ElementsThing[index] = -1
            localStorage.setItem("usages", JSON.stringify(ElementsThing))
            if (elements === 0) {
                timeout = setTimeout(function () {
                    if (elements === 0) {
                        background.className = "absolute top-1/2 bg-black opacity-60 transition duration-1000 w-screen h-screen left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
                        title.className = "absolute opacity-100 top-1/2 left-1/2 transform duration-1000 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
                    }
                }, 3000);
            }
        }
    }

    function ReloadElements() {
        elements = 0;
        if (localStorage.getItem("usages")) {
            ElementsThing = JSON.parse(String(localStorage.getItem("usages")));
            for (let i = 0; i < 15; i++) {
                if (ElementsThing[i] != -1) elements += 1
            }
        }
    }

    function mouseOver(index: number) {
        overElement = index
    }

    function mouseLeave() {
        overElement = -1
    }

    function Fullscreen(index:number) {
        ReloadElements()
        fullscreen = true 
        fullscreenID = ElementsThing[index]
    }

    onMount(() => {
        if(fullscreenID == -1) fullscreen = false
        if (localStorage.getItem("usages")) {
            ElementsThing = JSON.parse(String(localStorage.getItem("usages")));
            for (let i = 0; i < 15; i++) {
                if (ElementsThing[i] != -1) elements += 1
            }
        } else {
            for (let i = 0; i < 15; i++) {
                ElementsThing[i] = -1
            }
            localStorage.setItem("usages", JSON.stringify(ElementsThing))
        }
        loaded = true
        // Add the event listener to the document
        document.addEventListener('mousemove', handleMouseMove);
        timeout = setTimeout(function () {
            if (elements === 0) {
                background.className = "absolute top-1/2 bg-black opacity-60 transition duration-1000 w-screen h-screen left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
                title.className = "absolute opacity-100 top-1/2 left-1/2 transform duration-1000 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center"
            }
        }, 3000);
        clearTimeout(timeout)
    })
</script>

{#if fullscreen}
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
    class="bg-white-gray w-screen h-screen items-center justify-center"
    >
    <Element 
        index={-1} 
        bind:X 
        bind:Y 
        bind:Br 
        bind:Bl 
        bind:Fl 
        bind:Fr 
        element={fullscreenID} 
        ElementClass={"w-full h-full"}
    />
    <button 
        class="outline dark:text-white dark:outline-white outline-black opacity-25 hover:opacity-100 absolute top-24 right-2 hover:outline-2 outline-0 active:outline-1 transition ease-in dark:ease-out duration-100 p-1.5 h-8 w-8 rounded-md flex items-center justify-center" 
        on:click={()=>{fullscreen = false}}
    >
        <ArrowsAngleContract class="w-5 h-5" />
    </button>
    </div>
{:else}
    {#if elements === 0}
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div bind:this={background}
            class="absolute top-1/2 bg-black opacity-0 w-screen h-screen left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center">
        </div>
        <div bind:this={title} class="absolute opacity-0 top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center">
            <p class="dark:text-white text-xl">Add at least one element</p>
        </div>
    {/if}


    {#if loaded}
        <div class="flex flex-wrap mx-auto justify-center">
            {#each Array.from({ length: Math.min(5 * 3, ElementsThing.length) }) as _, index}
                {#if index < 15 && ElementsThing[index] == -1}
                    <ElementChooserGhost {index} Add={() => addElement(index)} Over={() => { mouseOver(index) }} Left={() => { mouseLeave() }} />
                {/if}
                {#if index < 15 && ElementsThing[index] != -1}
                    <ElementChooser element={ElementsThing[index]} bind:X bind:Y bind:Br bind:Bl bind:Fl bind:Fr bind:overElement {index} Reload={() => ReloadElements()} Remove={() => removeElement(index)} Over={() => { mouseOver(index) }} Left={() => { mouseLeave() }} Full={()=>{Fullscreen(index)}}/>
                {/if}
            {/each}
        </div>
    {/if}
{/if}
