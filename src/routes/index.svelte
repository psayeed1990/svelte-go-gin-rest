<script>
	import Menu from '../components/Menu.svelte';

	async function getUsers() {
		const res = await fetch(`http://psgqtnzaim.us08.qoddiapp.com/v1/users`);
		const { Users } = await res.json();

		if (res.ok) {
			return Users;
		} else {
			throw new Error(text);
		}
	}

	let promise = getUsers();
</script>

<Menu />
<h1>Our User Database For A Tutorial</h1>
<h2>List of Users</h2>
<ul>
	{#await promise}
		<p>...waiting</p>
	{:then Users}
		{#each Users as user}
			<li>
				<a href={user.id}>{user.name}</a>
			</li>
		{/each}
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}
</ul>
