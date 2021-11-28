<script context="module">
	//disable ssr
	export const router = false;
	export const prerender = false;
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
</script>

<Menu />
<ul>
	{#await promise}
		<p>...waiting</p>
	{:then User}
		<li>
			<a href={User.id}>{User.name}</a>
			<ul>
				<li>Email: {User.email}</li>
				<li>Password: {User.password}</li>
			</ul>
		</li>
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}
</ul>
