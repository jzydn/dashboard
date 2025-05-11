<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import { API_URL } from "../js/constants";
  import { notifyError, withLoadingScreen } from "../js/util";
  import { getIconUrl, getDefaultIcon } from "../js/icons";
  import ManageSidebarLink from "./ManageSidebarLink.svelte";

  export let currentRoute;
  export let permissionLevel;

  $: isAdmin = permissionLevel >= 2;

  let guildId = currentRoute.namedParams.id;
  let guild = {};
  let iconUrl = "";
  let retried = false;

  function handleIconLoadError() {
    if (!retried) {
      retried = true;
      iconUrl = getDefaultIcon(guildId);
    }
  }

  async function loadGuild() {
    try {
      const res = await axios.get(`${API_URL}/api/${guildId}/guild`);
      if (res.status !== 200) throw new Error(res.data.error);
      guild = res.data;
    } catch (err) {
      notifyError(err.message);
    }
  }

  onMount(async () => {
    await withLoadingScreen(async () => {
      await loadGuild();
      iconUrl = getIconUrl(guildId, guild.icon);
    });
  });
</script>

<section class="sidebar">
  <header>
    <img
      src="{iconUrl}"
      alt="Guild Icon"
      class="guild-icon"
      on:error={handleIconLoadError}
    />
    <span class="guild-name" title="{guild.name}">{guild.name}</span>
  </header>

  <nav>
    <ul class="nav-list">
      <ManageSidebarLink {currentRoute} title="← Back to servers" href="/" />

      {#if isAdmin}
        <ManageSidebarLink {currentRoute} title="Settings" icon="fa-cogs" href="/manage/{guildId}/settings" />
      {/if}

      <ManageSidebarLink {currentRoute} title="Transcripts" icon="fa-copy" href="/manage/{guildId}/transcripts" />

      {#if isAdmin}
        <ManageSidebarLink {currentRoute} routePrefix="/manage/{guildId}/panels" title="Ticket Panels" icon="fa-mouse-pointer" href="/manage/{guildId}/panels" />
        <ManageSidebarLink {currentRoute} title="Forms" icon="fa-poll-h" href="/manage/{guildId}/forms" />
        <ManageSidebarLink {currentRoute} title="Staff Teams" icon="fa-users" href="/manage/{guildId}/teams" />
        <ManageSidebarLink {currentRoute} title="Integrations" icon="fa-robot" href="/manage/{guildId}/integrations" />
      {/if}

      <ManageSidebarLink {currentRoute} title="Tickets" icon="fa-ticket-alt" href="/manage/{guildId}/tickets" />
      <ManageSidebarLink {currentRoute} title="Blacklist" icon="fa-ban" href="/manage/{guildId}/blacklist" />
      <ManageSidebarLink {currentRoute} title="Tags" icon="fa-tags" href="/manage/{guildId}/tags" />
    </ul>
  </nav>

  <nav class="bottom">
    <hr />
    <ul class="nav-list">
      <ManageSidebarLink {currentRoute} title="Documentation" icon="fa-book" href="https://docs.ticketsbot.net" newWindow />
      <ManageSidebarLink {currentRoute} title="Logout" icon="fa-sign-out-alt" href="/logout" />
    </ul>
  </nav>
</section>

<style>
  .sidebar {
    display: flex;
    flex-direction: column;
    align-self: flex-start;
    background: rgba(39, 39, 39, 0.7);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border: 1px solid rgba(255, 255, 255, 0.08);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
    padding: 15px;
    width: 275px;
    border-radius: 12px;
    user-select: none;
    transition: all 0.2s ease-in-out;
  }

  header {
    display: flex;
    align-items: center;
    gap: 12px;
    background: rgba(255, 255, 255, 0.06);
    padding: 10px 12px;
    border-radius: 10px;
    font-weight: 600;
    color: #fff;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }

  .guild-icon {
    width: 44px;
    height: 44px;
    border-radius: 50%;
    border: 2px solid rgba(255, 255, 255, 0.12);
    object-fit: cover;
    background-color: #1a1a1a;
  }

  .guild-name {
    font-size: 15px;
    max-width: 160px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }

  nav > ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  nav hr {
    width: 40%;
    margin-left: 15px;
    border-color: rgba(255, 255, 255, 0.1);
  }

  @media (max-width: 800px) {
    .sidebar {
      display: none;
    }
  }
</style>
