<script lang="ts">
    import axios from 'axios'; // Import axios for making HTTP requests
    import { onDestroy, onMount } from 'svelte';
    import "../app.css";
    import Header from "$lib/components/Header.svelte";
	import { getUrlWithNoPort } from '$lib/logic/main';
    import WebSocketManager from '$lib/logic/WebSocketManager';
    import { getIpWithoutProtocol } from '$lib/logic/main';
    
    const webSocketManager = WebSocketManager.getInstance();
    var logged = true;
    var permission:boolean = false
    var device = ""
</script>

{#if logged}
    <Header/>
    <slot></slot>
    {#if device != ""}
        <div class="fixed z-10 inset-0 overflow-y-auto">
            <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
                <div class="fixed inset-0 transition-opacity" aria-hidden="true">
                    <div class="absolute inset-0 opacity-75"></div>
                </div>

                <!-- This element is to trick the browser into centering the modal contents. -->
                <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>

                <div class="inline-block align-bottom bg-gray rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
                    <div class="bg-gray dark:bg-black-gray px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                        <div class="sm:flex sm:items-start">
                            <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                                <h3 class="dark:text-white text-lg leading-6 font-medium text-gray-900" id="modal-headline">
                                    Control request
                                </h3>
                                <div class="mt-2">
                                    <p class="dark:text-white text-sm text-gray-500">
                                        Device: {device}
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="dark:bg-mid-black-gray bg-white-gray px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
                        <button type="button" class="mb-2 dark:bg-black-gray bg-mid-gray w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 text-base font-medium dark:text-white ring-2 ring-gray dark:ring-white hover:bg-green hover:dark:bg-green sm:ml-3 sm:w-auto sm:text-sm">
                            Accept
                        </button>
                        <button type="button" class="mb-2 dark:bg-black-gray bg-mid-gray w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 text-base font-medium dark:text-white ring-2 ring-gray dark:ring-white hover:bg-red hover:dark:bg-red sm:ml-3 sm:w-auto sm:text-sm">
                            Deny
                        </button>
                    </div>
                </div>
            </div>
        </div>
    {/if}


{/if}