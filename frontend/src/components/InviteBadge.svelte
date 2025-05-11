<script>
  function invite() {
    window.location.href = `https://discord.com/oauth2/authorize?client_id=1370511947459465237&permissions=8&integration_type=0&scope=bot`;
  }
</script>

<style>
  .guild-badge {
    display: flex;
    align-items: center;
    background-color: #2c2f33;
    border: 2px solid #7289da;
    border-radius: 12px;
    padding: 12px 16px;
    cursor: pointer;
    transition: background-color 0.2s ease, transform 0.2s ease;
    max-width: 400px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.25);
  }

  .guild-badge:hover {
    background-color: #3a3d41;
    transform: scale(1.02);
  }

  .guild-icon-bg {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #7289da;
    border-radius: 50%;
    width: 48px;
    height: 48px;
    color: white;
    flex-shrink: 0;
  }

  .guild-name {
    font-size: 1rem;
    font-weight: 600;
    color: white;
    margin-left: 12px;
  }
</style>

<div class="guild-badge" on:click={invite}>
  <div class="guild-icon-bg">
    <i class="fas fa-plus fa-lg"></i>
  </div>
  <span class="guild-name">Add Otter Tickets to your server</span>
</div>
