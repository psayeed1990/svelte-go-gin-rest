<script>
	import Menu from '../components/Menu.svelte';

	let name;
	let email;
	let password;

	//async createUser function to create a new user with fetch POST request
	async function createUser() {
		console.log('he');
		const response = await fetch('http://psgqtnzaim.us08.qoddiapp.com/v1/users', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				name: name,
				email: email,
				password: password
			})
		});
		const body = await response.json();
		if (response.status !== 200) throw Error(body.message);
		console.log(body);
		return body;
	}
</script>

<Menu />
<form on:submit|preventDefault={createUser}>
	<label for="name">Name</label><br />
	<input bind:value={name} />
	<br />
	<label for="email">Email</label><br />
	<input bind:value={email} />
	<br />
	<label for="password">Password</label><br />
	<input type="password" bind:value={password} />
	<br />
	<button>Submit</button>
</form>
