<script lang="ts">
	import { stringify } from 'postcss';
    import Clock from './Clock.svelte';
    import DayChart from './DayChart.svelte';
    import Joystick from './Joystick.svelte';
    import Theme from './Theme.svelte';
    import Webcam from './Webcam.svelte';
    import WheelsChart from './WheelsChart.svelte';
    import WifiSignal from './WifiSignal.svelte';
    import XyChart from './XYChart.svelte';
	import { onMount } from 'svelte';

    export let X: number, Y: number, Br: number, Bl: number, Fl: number, Fr: number;
    let permission: boolean = false;

    let writes: string[] = ["Clock", "DayChart", "Joystick", "Theme", "Webcam", "WheelsChart", "WifiSignal", "XyChart"];
    export let element: number;
    export let index: number;
    export let ElementClass: string;
    export let updateElementChooser = () =>{};
    function added() {
        console.log(index)
        updateElementChooser()
    }
</script>


<div class={ElementClass}>
    {#if element == 0}
        <div class="max-h-40 min-w-40 overflow-scroll">
            {#each writes as word, i}
                <div class="text-center">
                    <button class="outline outline-0 hover:outline-2 dark:outline-white dark:text-white active:outline-1 my-2 rounded-md" on:click={() => {
                        element = i + 1;
                        let ElementsThing = JSON.parse(String(localStorage.getItem("usages")));
                        ElementsThing[index] = element;
                        localStorage.setItem("usages", JSON.stringify(ElementsThing));
                        added()
                    }}>
                        <p class="m-1">{word}</p>
                    </button>
                </div>
            {/each}
        </div>
    {/if}

    {#if element == 1}
        <div class="mx-auto dark:text-white">
            <Clock />
        </div>
    {/if}

    {#if element == 2}
        <div class="mx-auto dark:text-white">
            <DayChart />
        </div>
    {/if}

    {#if element == 3}
        <div class="mx-auto dark:text-white">
            <Joystick bind:permission bind:X bind:Y bind:Br bind:Bl bind:Fl bind:Fr />
        </div>
    {/if}

    {#if element == 4}
        <div class="mx-auto dark:text-white">
            <Theme />
        </div>
    {/if}

    {#if element == 5}
        <div class="mx-auto dark:text-white">
            <Webcam className=""/>
        </div>
    {/if}

    {#if element == 6}
        <div class="mx-auto dark:text-white">
            <WheelsChart bind:Br bind:Bl bind:Fl bind:Fr />
        </div>
    {/if}

    {#if element == 7}
        <div class="mx-auto dark:text-white">
            <WifiSignal />
        </div>
    {/if}

    {#if element == 8}
        <div class="mx-auto dark:text-white">
            <XyChart bind:X bind:Y />
        </div>
    {/if}
</div>