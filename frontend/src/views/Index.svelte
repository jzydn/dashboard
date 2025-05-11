<svelte:head>
  <!-- Prevent zooming / font scaling on mobile -->
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
</svelte:head>

<div class="content">
  <div class="card-wrapper">
    <Card footer={false} fill={false}>
      <span slot="title">Servers</span>

      <div slot="body" style="width: 100%">
        <div id="guild-container">
          <InviteBadge/>

          {#each guilds as guild}
            <Guild guild={guild}/>
          {/each}
        </div>

        <div class="flex-container" id="refresh-container">
          <Button icon="fas fa-sync" on:click={refreshGuilds}>
            Refresh list
          </Button>
        </div>
      </div>
    </Card>
  </div>
</div>

<script>
  import axios from 'axios';
  import { notifyError, withLoadingScreen } from '../js/util';
  import { setDefaultHeaders } from '../includes/Auth.svelte';
  import { API_URL } from "../js/constants.js";
  import Guild from '../components/Guild.svelte';
  import Card from '../components/Card.svelte';
  import InviteBadge from '../components/InviteBadge.svelte';
  import Button from '../components/Button.svelte';
  import { loadingScreen } from "../js/stores";

  setDefaultHeaders();

  let guilds = window.localStorage.getItem('guilds')
    ? JSON.parse(window.localStorage.getItem('guilds'))
    : [];

  async function refreshGuilds() {
    await withLoadingScreen(async () => {
      const res = await axios.post(`${API_URL}/user/guilds/reload`);
      if (res.status !== 200) {
        notifyError(res.data.error);
        return;
      }

      if (!res.data.success && res.data['reauthenticate_required'] === true) {
        window.location.href = "/login";
        return;
      }

      guilds = res.data.guilds;
      window.localStorage.setItem('guilds', JSON.stringify(guilds));
    });
  }

  loadingScreen.set(false);
</script>

<style>
  .content {
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100%;
    padding: 1rem;
    box-sizing: border-box;
  }

  .card-wrapper {
    width: 100%;
    max-width: 1100px;
  }

  #guild-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 16px;
  }

  #refresh-container {
    display: flex;
    justify-content: center;
    margin: 16px 0;
    color: white;
  }

  @media (max-width: 576px) {
    .card-wrapper {
      width: 100%;
    }

    #guild-container {
      flex-direction: column;
      align-items: center;
    }
  }
</style>
