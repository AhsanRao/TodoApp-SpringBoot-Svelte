<script>
	import { onMount } from 'svelte';
	let message = 'Loading...';
	  
	onMount(async () => {
    try {
      const response = await fetch('http://localhost:8080/hello');
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      message = await response.text();
    } catch (e) {
      console.log(e);
      message = 'Failed to load message';
    }
  });
  let username = '';
  let password = '';

  async function handleSubmit() {
    try {
      const response = await fetch('http://localhost:8080/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password }),
      });

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      console.log('Data sent to the backend:', { username, password });
      const result = await response.text();
      console.log('Response from the backend:', result);
    } catch (error) {
      console.error('Error sending data to the backend:', error);
    }
  }
</script>

<main>
	<h1>{message}</h1>
	<form on:submit|preventDefault={handleSubmit}>
		<div>
		  <label for="username">Username:</label>
		  <input id="username" type="text" bind:value={username} />
		</div>
		<div>
		  <label for="password">Password:</label>
		  <input id="password" type="password" bind:value={password} />
		</div>
		<div>
		  <button type="submit">Login</button>
		</div>
	  </form>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>