<Head/>

<div class="wrapper">
  <Sidebar {userData} />
  <div class="super-container">
    {#if currentRoute.component !== Index}
      <LoadingScreen/>
    {/if}
    <NotifyModal/>
    <div class="content-container" class:hide={$loadingScreen}>
      <Route {currentRoute} {params}/>
    </div>
  </div>
</div>

<script>
  import { Route } from 'svelte-router-spa'
  import Head from '../includes/Head.svelte'
  import Sidebar from '../includes/Sidebar.svelte'
  import LoadingScreen from '../includes/LoadingScreen.svelte'
  import NotifyModal from '../includes/NotifyModal.svelte'
  import { loadingScreen } from "../js/stores"
  import { redirectLogin, setDefaultHeaders } from '../includes/Auth.svelte'
  import { onMount } from "svelte";
  import Index from "../views/Index.svelte";

  export let currentRoute;
  export let params = {};

  setDefaultHeaders()

  let userData = {
    id: 0,
    username: 'Unknown',
    avatar: '',
    admin: false
  };

  onMount(() => {
    if (!window.localStorage.getItem('user_data') || !window.localStorage.getItem('guilds')) {
      redirectLogin();
      return;
    }

    const retrieved = window.localStorage.getItem('user_data');
    if (retrieved) {
      userData = JSON.parse(retrieved);
    }
  });
</script>

<style>
  :global(body) {
    margin: 0;
    font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    background-color: #0f0f12;
    color: #f1f1f1;
    padding: 0 !important;
    overflow: hidden;
  }

  .wrapper {
    display: flex;
    width: 100%;
    height: 100%;
    margin: 0 !important;
    padding: 0 !important;
    background: linear-gradient(to bottom right, #0f0f12, #1a1a1e);
  }

  .super-container {
    display: flex;
    width: 100%;
    height: 100%;
    background-color: rgba(255, 255, 255, 0.02);
    backdrop-filter: blur(8px);
    border-left: 1px solid rgba(255, 255, 255, 0.05);
  }

  .content-container {
    display: flex;
    width: 100%;
    height: 100%;
    padding: 1rem;
    box-sizing: border-box;
    overflow-y: auto;
    background-color: rgba(255, 255, 255, 0.015);
    border-radius: 1rem;
    box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.025);
    transition: opacity 0.25s ease-in-out;
  }

  .hide {
    visibility: hidden;
    opacity: 0;
  }

  @media (max-width: 950px) {
    .wrapper {
      flex-direction: column;
    }

    .content-container {
      border-radius: 0;
    }
  }

  /* Optional: smoother scrollbar for modern look */
  :global(*) {
    scrollbar-width: thin;
    scrollbar-color: #444 #1a1a1e;
  }

  :global(*::-webkit-scrollbar) {
    width: 8px;
    height: 8px;
  }

  :global(*::-webkit-scrollbar-thumb) {
    background: #444;
    border-radius: 4px;
  }

  :global(*::-webkit-scrollbar-track) {
    background: #1a1a1e;
  }
</style>
