<script>
    import { onMount } from 'svelte';

    let users = [];
    let loading = true;
    let error = '';

    // Fetching data when the component is mounted
    onMount(async () => {
        try {
            const response = await fetch('https://jsonplaceholder.typicode.com/users');
            if (!response.ok) {
                throw new Error('Failed to fetch users');
            }
            users = await response.json();
        } catch (err) {
            error = err.message;
        } finally {
            loading = false;
        }
    });
</script>

<main>
    <h1>Users List</h1>
    {#if loading}
        <p>Loading...</p>
    {:else if error}
        <p style="color: red;">{error}</p>
    {:else}
        <ul>
            {#each users as user}
                <li>
                    <strong>{user.name}</strong> ({user.email})
                    <br />
                    <small>{user.address.city}, {user.address.street}</small>
                </li>
            {/each}
        </ul>
    {/if}
</main>

<style>
    main {
        max-width: 600px;
        margin: 50px auto;
        padding: 20px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        border-radius: 8px;
        background-color: #f9f9f9;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        margin: 10px 0;
        padding: 10px;
        border-bottom: 1px solid #ddd;
    }
</style>
