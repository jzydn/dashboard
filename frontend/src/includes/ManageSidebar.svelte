<script>
  import { onMount } from "svelte";
  import { fade } from "svelte/transition";
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
    if (retried) return;
    retried = true;
    iconUrl = getDefaultIcon(guildId);
  }

  async function loadGuild() {
    try {
      const res = await axios.get(`${API_URL}/api/${guildId}/guild`);
      if (res.status === 200) {
        guild = res.data;
        iconUrl = getIconUrl(guildId, guild.icon);
      } else {
        notifyError(res.data.error || "Failed to load guild.");
      }
    } catch (err) {
      notifyError("An error occurred while loading the guild.");
    }
  }

  onMount(() => {
    withLoadingScreen(loadGuild);
  });
</script>

<section class="sidebar" transition:fade={{ duration: 250 }}>
  <header>
    <img
      src={iconUrl}
      class="guild-icon"
      alt="Guild icon"
      width="50"
      height="50"
      on:error={handleIconLoadError}
    />
    <span>{guild.name}</span>
  </header>

  <nav>
    <ul class="nav-list">
      <ManageSidebarLink {currentRoute} title="← Back to servers" href="/" />

      {#if isAdmin}
        <ManageSidebarLink {currentRoute} title="Settings" icon="fa-cogs" href={`/manage/${guildId}/settings`} />
      {/if}

      <ManageSidebarLink {currentRoute} title="Transcripts" icon="fa-copy" href={`/manage/${guildId}/transcripts`} />

      {#if isAdmin}
        <ManageSidebarLink
          {currentRoute}
          routePrefix={`/manage/${guildId}/panels`}
          title="Ticket Panels"
          icon="fa-mouse-pointer"
          href={`/manage/${guildId}/panels`}
        />

        <ManageSidebarLink {currentRoute} title="Forms" icon="fa-poll-h" href={`/manage/${guildId}/forms`} />
        <ManageSidebarLink {currentRoute} title="Staff Teams" icon="fa-users" href={`/manage/${guildId}/teams`} />
        <ManageSidebarLink {currentRoute} title="Integrations" icon="fa-robot" href={`/manage/${guildId}/integrations`} />
      {/if}

      <ManageSidebarLink {currentRoute} title="Tickets" icon="fa-ticket-alt" href={`/manage/${guildId}/tickets`} />
      <ManageSidebarLink {currentRoute} title="Blacklist" icon="fa-ban" href={`/manage/${guildId}/blacklist`} />
      <ManageSidebarLink {currentRoute} title="Tags" icon="fa-tags" href={`/manage/${guildId}/tags`} />
    </ul>
  </nav>

  <nav class="bottom">
    <hr />
    <ul class="nav-list">
      <ManageSidebarLink
        {currentRoute}
        title="Documentation"
        icon="fa-book"
        href="https://docs.ticketsbot.net"
        newWindow
      />
      <ManageSidebarLink {currentRoute} title="Logout" icon="fa-sign-out-alt" href="/logout" />
    </ul>
  </nav>
</section>
