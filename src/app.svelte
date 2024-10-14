<script>
	import Checkbox from './checkbox.svelte';

	let state = $state({})
	
	const schema = $state({
    properties: {
      foo: true,
    },
  })
	
	function retrieveSchema(schema, value) {
		const cloned = { ...schema, properties: { ...schema.properties } }
		for (const key of Object.keys(value)) {
			cloned.properties[key] = key
		}
		return cloned
	}

	const retrieved = $derived(retrieveSchema(schema, state));
	const required = $derived(new Set(retrieved.required));
	const properties = $derived(retrieved.properties);
	const keys = $derived(Object.keys(properties));
	let nextKey = 1;
</script>

<button onclick={() => {
	state[nextKey++] = true
}}>
	Add property
</button>

{#each keys as key (key)}
	{@const config = { title: key, required: required.has(key) }}
	<Checkbox bind:value={state[key]} {config} />
{/each}
