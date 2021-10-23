<script lang="ts">
	import type { FakeUserRes } from 'src/types/fakeUser';
	let name: string = 'Liran';
	const fetchUser = () =>
		fetch('https://randomuser.me/api').then(async (res) => {
			const result: FakeUserRes | null = await res.json().catch((err) => null);
			if (!result) return result;
			result.results[0].dob.date = new Date(result.results[0].dob.date);
			return result.results[0];
		});
	let fakeUserRes = fetchUser();
</script>

<input bind:value={name} />
<h1>Hello, {name}!</h1>
{#await fakeUserRes}
	<h3>Loading...</h3>
{:then fakeUser}
	{#if fakeUser}
		<ul>
			<li>{`Name: ${fakeUser.name.first} ${fakeUser.name.last}`}</li>
			<li>{`Date of birth: ${fakeUser.dob.date.toLocaleDateString()}`}</li>
		</ul>
	{:else}
		<h1 class="red">Error loading user!</h1>
	{/if}
{:catch err}
	<h1 class="red">Error loading user!</h1>
{/await}
<button
	on:click={() => {
		fakeUserRes = fetchUser();
	}}>Get New User</button
>

<style>
	.red {
		color: red;
	}
</style>
