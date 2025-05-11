<script>
  import { Navigate } from 'svelte-router-spa';
  import { getAvatarUrl, getDefaultIcon } from "../js/icons";

  export let userData;

  let hasFailed = false;
  function handleAvatarLoadError(e, userId) {
    if (!hasFailed) {
      hasFailed = true;
      e.target.src = getDefaultIcon(userId);
    }
  }

  function clearLocalStorage() {
    localStorage.clear();
  }
</script>

<div class="sidebar">
  <div class="nav-group">
    <Navigate to="/" class="nav-link">
      <i class="fas fa-server"></i>
      <span>Servers</span>
    </Navigate>
    <Navigate to="/whitelabel" class="nav-link">
      <i class="fas fa-edit"></i>
      <span>Whitelabel</span>
    </Navigate>
    {#if userData.admin}
      <Navigate to="/admin/bot-staff" class="nav-link">
        <i class="fa-solid fa-user-secret"></i>
        <span>Admin</span>
      </Navigate>
    {/if}
  </div>

  <div class="nav-group bottom">
    <Navigate to="/logout" class="nav-link" on:click={clearLocalStorage}>
      <i class="fas fa-sign-out-alt"></i>
      <span>Logout</span>
    </Navigate>

    <div class="user-profile">
      <img
        class="avatar"
        src={getAvatarUrl(userData.id, userData.avatar)}
        alt="Avatar"
        on:error={(e) => handleAvatarLoadError(e, userData.id)}
      />
      <span>{userData.username}</span>
    </div>
  </div>
</div>

<style>
  .sidebar {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    background-color: #1e1f22;
    color: white;
    height: 100vh;
    width: 260px;
    padding: 20px 0;
    box-shadow: 4px 0 12px rgba(0, 0, 0, 0.15);
    overflow: hidden;
  }

  .nav-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
    padding: 0 20px;
  }

  .nav-group.bottom {
    margin-top: auto;
  }

  .nav-link {
    display: flex;
    align-items: center;
    padding: 12px;
    border-radius: 8px;
    color: white;
    text-decoration: none;
    font-size: 16px;
    transition: background 0.2s ease, transform 0.1s ease;
  }

  .nav-link i {
    margin-right: 10px;
    font-size: 18px;
    width: 22px;
    text-align: center;
  }

  .nav-link:hover {
    background-color: #2b2d31;
    transform: scale(1.02);
  }

  .user-profile {
    display: flex;
    align-items: center;
    padding: 12px 20px;
    gap: 10px;
    border-top: 1px solid #2e2f33;
    margin-top: 12px;
  }

  .user-profile .avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    object-fit: cover;
    background-color: #444;
  }

  .user-profile span {
    font-weight: 500;
    font-size: 15px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  @media (max-width: 950px) {
    .sidebar {
      flex-direction: row;
      height: auto;
      width: 100%;
      padding: 10px;
    }

    .nav-group {
      flex-direction: row;
      gap: 12px;
      padding: 0;
      align-items: center;
    }

    .nav-link {
      font-size: 0;
      padding: 10px;
    }

    .nav-link i {
      font-size: 20px;
      margin: 0;
    }

    .user-profile {
      display: none;
    }
  }
</style>
