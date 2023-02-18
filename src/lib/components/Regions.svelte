<script lang="ts">
	import { Region as api } from '$lib/data/region'
	import { onMount } from 'svelte'
	let promise: any = Promise.resolve()

	export let value: number
	export let name: string = 'Region'

	let items: any = []

	onMount(async () => {
		promise = await api.fetchAll()
		items = promise.resource
	})
</script>

{#await promise}
	<p>Loading ...</p>
{:then}
	{#if items}
		<label for={name}>Region</label>
		<select {name} id={name}>
			<option value="" />
			{#each items as { RegionId, RegionDescription }}
				{#if RegionId === value}
					<option value={RegionId} selected>{RegionDescription}</option>
				{:else}
					<option value={RegionId}>{RegionDescription}</option>
				{/if}
			{/each}
		</select>
	{/if}
{:catch error}
	<p>Something went wrong: {error.message}</p>
{/await}
