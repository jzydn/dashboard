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
:global(html),
:global(body) {
  height: 100%;
  overflow: hidden; /* Keep this on body, but ensure the scrollable container handles it */
}

.wrapper {
  height: 100vh; /* Ensures it fills the full viewport height */
  overflow: hidden;
}

.super-container {
  height: 100vh;
  overflow: hidden;
  flex: 1;
  display: flex;
  flex-direction: column; /* Needed if content is vertical */
}

.content-container {
  flex: 1;
  overflow-y: auto; /* Enables vertical scrolling */
  overflow-x: hidden;
  padding: 1rem;
  box-sizing: border-box;
  background-color: rgba(255, 255, 255, 0.015);
  border-radius: 1rem;
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.025);
  transition: opacity 0.25s ease-in-out;
  min-height: 0; /* Prevents overflow bugs in flexbox children */
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
