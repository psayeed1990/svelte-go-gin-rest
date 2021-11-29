<script context="module">
	//svelte dynamic route slug
	export async function load(ctx) {
		let id = ctx.page.params.id;
		return { props: { id } };
	}
</script>

<script>
	import Menu from '../components/Menu.svelte';

	export let id;
	//svelte load function to fetch data
	async function getUser() {
		const res = await fetch(`http://psgqtnzaim.us08.qoddiapp.com/v1/users/${id}`);
		const { User } = await res.json();

		if (res.ok) {
			return User;
		} else {
			throw new Error(text);
		}
	}

	const promise = getUser();

	//delete user
	let deleting = false;
	let msg = '';
	let deleted = false;
	async function deleteUser() {
		deleting = true;
		msg = 'Deleting...';
		const res = await fetch(`http://psgqtnzaim.us08.qoddiapp.com/v1/users/${id}`, {
			method: 'DELETE',
			headers: {
				'Content-Type': 'application/json'
			}
		});
		const { User } = await res.json();

		if (res.ok) {
			msg = 'User deleted';
			deleted = true;
			return (window.location.href = '/');
		} else {
			throw new Error(text);
		}
	}

	//content editable
	let name;
	let email;
	let password;
	let updating = false;

	async function updateUser() {
		updating = true;
		msg = 'Updating...';
		const res = await fetch(`http://psgqtnzaim.us08.qoddiapp.com/v1/users/${id}`, {
			method: 'PUT',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				name,
				email,
				password
			})
		});
		const { User } = await res.json();
		if (res.ok) {
			msg = 'User Updated';
			updating = false;
			return User;
		} else {
			throw new Error(text);
		}
	}
</script>

<Menu />
<ul>
	{#await promise}
		<p>...waiting</p>
	{:then User}
		<li>
			<a href={User.id}>{User.id}</a>
			<ul>
				{#if !deleted}
					<li>Name: <span contenteditable="true" bind:innerHTML={name}>{User.name}</span></li>
					<li>Email: <span contenteditable="true" bind:innerHTML={email}>{User.email}</span></li>
					<li>
						Password: <span contenteditable="true" bind:innerHTML={password}>{User.password}</span>
					</li>
				{/if}

				{#if !updating}
					<li class="update" on:click={updateUser}>Update {User.name}</li>
				{:else}
					<li>...{msg}</li>
				{/if}

				{#if !deleting}
					<li class="delete" on:click={deleteUser}>Delete {User.name}</li>
				{:else}
					<li>...{msg}</li>
				{/if}
			</ul>
		</li>
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}
</ul>

<style>
	.delete {
		cursor: pointer;
		background-color: brown;
		color: wheat;
		width: max-content;
		padding: 5px;
	}
	.update {
		cursor: pointer;
		background-color: green;
		color: wheat;
		width: max-content;
		padding: 5px;
	}
</style>
