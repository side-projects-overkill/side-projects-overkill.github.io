<script>
  import { onMount } from 'svelte';
  let repos = [];
  let loading = true;
  let error = '';

  onMount(async () => {
    try {
      const res = await fetch('https://api.github.com/orgs/side-projects-overkill/repos?per_page=100');
      if (!res.ok) throw new Error('Failed to fetch repos');
      repos = await res.json();
    } catch (e) {
      error = e.message;
    } finally {
      loading = false;
    }
  });
</script>

<main class="landing">
  <h1>🚀 Side Projects Overkill</h1>
  <p class="subtitle">A collection of awesome side projects from the <a href="https://github.com/side-projects-overkill" target="_blank">side-projects-overkill</a> GitHub organization.</p>
  {#if loading}
    <div class="loader">Loading repositories...</div>
  {:else if error}
    <div class="error">{error}</div>
  {:else}
    <div class="repo-list">
      {#each repos as repo}
        <a class="repo-card" href={repo.html_url} target="_blank">
          <h2>{repo.name}</h2>
          <p>{repo.description || 'No description provided.'}</p>
          <div class="repo-meta">
            <span>⭐ {repo.stargazers_count}</span>
            <span>🍴 {repo.forks_count}</span>
            {#if repo.language}
              <span>💻 {repo.language}</span>
            {/if}
          </div>
        </a>
      {/each}
    </div>
  {/if}
</main>

<style>
.landing {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}
.subtitle {
  color: #888;
  margin-bottom: 2rem;
}
.loader {
  font-size: 1.2rem;
  color: #646cff;
}
.error {
  color: #ff3e00;
  font-weight: bold;
}
.repo-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1.5rem;
  margin-top: 2rem;
}
.repo-card {
  background: linear-gradient(135deg, #f9f9f9 60%, #e0e7ff 100%);
  border-radius: 1rem;
  box-shadow: 0 4px 24px 0 rgba(100, 108, 255, 0.08);
  padding: 1.5rem;
  text-align: left;
  text-decoration: none;
  color: #213547;
  transition: transform 0.15s, box-shadow 0.15s;
  display: flex;
  flex-direction: column;
  min-height: 160px;
}
.repo-card:hover {
  transform: translateY(-4px) scale(1.03);
  box-shadow: 0 8px 32px 0 rgba(100, 108, 255, 0.16);
}
.repo-card h2 {
  margin: 0 0 0.5rem 0;
  font-size: 1.3rem;
  color: #646cff;
}
.repo-card p {
  flex: 1;
  margin: 0 0 1rem 0;
  color: #555;
}
.repo-meta {
  display: flex;
  gap: 1.2rem;
  font-size: 0.95rem;
  color: #888;
}
</style> 