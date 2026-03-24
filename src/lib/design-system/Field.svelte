<script lang="ts">
	type FieldElement = 'input' | 'textarea';

	let {
		as = 'input',
		id,
		name,
		type = 'text',
		rows = 3,
		placeholder = '',
		class: className = ''
	}: {
		as?: FieldElement;
		id: string;
		name: string;
		type?: string;
		rows?: number;
		placeholder?: string;
		class?: string;
	} = $props();
</script>

{#if as === 'textarea'}
	<textarea
		{id}
		{name}
		{rows}
		{placeholder}
		class={['ds-field', className].filter(Boolean).join(' ')}
	></textarea>
{:else}
	<input {id} {name} {type} {placeholder} class={['ds-field', className].filter(Boolean).join(' ')} />
{/if}
