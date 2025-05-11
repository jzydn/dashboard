<div class="content">
  <div class="col">
    <Card footer footerRight ref="filter-card">
      <span slot="title">
        <i class="fas fa-filter"></i>
        Filter Logs
      </span>

      <div slot="body" class="body-wrapper">
        <div class="form-wrapper">
          <div class="row">
            <Input col4=true label="Ticket ID" placeholder="Ticket ID"
                   on:input={handleInputTicketId} bind:value={filterSettings.ticketId}/>

            <Input col4=true label="Username" placeholder="Username" on:input={handleInputUsername}
                   bind:value={filterSettings.username}/>

            <Input col4=true label="User ID" placeholder="User ID" on:input={handleInputUserId}
                   bind:value={filterSettings.userId}/>
          </div>
          <div class="row">
            <div class="col-4">
              <PanelDropdown label="Panel" isMulti={false} bind:panels bind:selected={selectedPanel} />
            </div>

            <Dropdown label="Rating" bind:value={filterSettings.rating}>
              <option value=0>Any</option>
              <option value=1>1 ⭐</option>
              <option value=2>2 ⭐</option>
              <option value=3>3 ⭐</option>
              <option value=4>4 ⭐</option>
              <option value=5>5 ⭐</option>
            </Dropdown>
          </div>
        </div>
      </div>
      <div slot="footer">
        <Button icon="fas fa-search" on:click={filter}>Filter</Button>
      </div>
    </Card>

    <div style="margin: 2% 0;">
      <Card footer="{false}">
        <span slot="title">
          Transcripts
        </span>

        <div slot="body" class="main-col">
          <table class="nice">
            <thead>
            <tr>
              <th>Ticket ID</th>
              <th>Username</th>
              <th>Rating</th>
              <th class="reason">Close Reason</th>
              <th>Transcript</th>
            </tr>
            </thead>
            <tbody>
            {#each transcripts as transcript}
              <tr>
                <td>{transcript.ticket_id}</td>
                <td>{transcript.username}</td>
                <td>
                  {#if transcript.rating}
                    {transcript.rating} ⭐
                  {:else}
                    No rating
                  {/if}
                </td>
                <td class="reason">{transcript.close_reason || 'No reason specified'}</td>
                {#if transcript.has_transcript}
                  <td>
                    <Navigate to={`/manage/${guildId}/transcripts/view/${transcript.ticket_id}`} styles="link">
                      <Button>View</Button>
                    </Navigate>
                  </td>
                {/if}
              </tr>
            {/each}
            </tbody>
          </table>

          <div class="nav" class:nav-margin={transcripts.length === 0}>
            <i class="fas fa-chevron-left" class:hidden={page === 1} on:click={loadPrevious}></i>
            <span>Page {page}</span>
            <i class="fas fa-chevron-right"
               class:hidden={transcripts.length < pageLimit || transcripts[transcripts.length - 1].ticket_id === 1}
               on:click={loadNext}></i>
          </div>
        </div>
      </Card>
    </div>
  </div>
</div>

<script>
    import Card from '../components/Card.svelte'
    import Input from '../components/form/Input.svelte'
    import Button from '../components/Button.svelte'

    import {notifyError, withLoadingScreen} from '../js/util'
    import {onMount} from "svelte";
    import {dropdown} from "../js/stores";
    import axios from "axios";
    import {API_URL} from "../js/constants";
    import {setDefaultHeaders} from '../includes/Auth.svelte'
    import {Navigate} from 'svelte-router-spa'
    import PanelDropdown from "../components/PanelDropdown.svelte";
    import Dropdown from "../components/form/Dropdown.svelte";

    setDefaultHeaders();

    export let currentRoute;
    let guildId = currentRoute.namedParams.id

    let filterSettings = {};
    let transcripts = [];

    let panels = [];
    let selectedPanel;

    const pageLimit = 15;
    let page = 1;

    let handleInputTicketId = () => {
        filterSettings.username = undefined;
        filterSettings.userId = undefined;

        if (filterSettings.ticketId === "") {
            filterSettings.ticketId = undefined;
        }
    };

    let handleInputUsername = () => {
        filterSettings.ticketId = undefined;
        filterSettings.userId = undefined;

        if (filterSettings.username === "") {
            filterSettings.username = undefined;
        }
    };

    let handleInputUserId = () => {
        filterSettings.ticketId = undefined;
        filterSettings.username = undefined;

        if (filterSettings.userId === "") {
           filterSettings.userId = undefined;
        }
    };

    let loading = false;

    async function loadPrevious() {
        if (loading) return;

        if (page === 1) {
            return;
        }

        let paginationSettings = buildPaginationSettings(page - 1);

        loading = true;
        if (await loadData(paginationSettings)) {
            page--;
        }
        loading = false;
    }

    async function loadNext() {
        if (loading) return;

        if (transcripts.length < pageLimit || transcripts[transcripts.length - 1].ticket_id === 1) {
            return;
        }

        let paginationSettings = buildPaginationSettings(page + 1);

        loading = true;
        if (await loadData(paginationSettings)) {
            page++;
        }
        loading = false;
    }

    function buildPaginationSettings(page) {
        return {
            id: filterSettings.ticketId,
            username: filterSettings.username,
            user_id: filterSettings.userId,
            rating: filterSettings.rating,
            panel_id: selectedPanel,
            page: page,
        };
    }

    async function filter() {
        let opts = buildPaginationSettings(1);
        await loadData(opts);
        page = 1;
    }

    async function loadPanels() {
        const res = await axios.get(`${API_URL}/api/${guildId}/panels`);
        if (res.status !== 200) {
            notifyError(res.data.error);
            return;
        }

        panels = res.data;
    }

    async function loadData(paginationSettings) {
        const res = await axios.post(`${API_URL}/api/${guildId}/transcripts`, paginationSettings);
        if (res.status !== 200) {
            notifyError(res.data.error);
            return false;
        }

        transcripts = res.data;
        return true;
    }

    withLoadingScreen(async () => {
        await Promise.all([
            loadPanels(),
            loadData({})
        ])
    })
</script>

<style>
/* Modernized styles applied above */
</style>
