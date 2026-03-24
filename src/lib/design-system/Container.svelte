<script lang="ts">
	import type { Snippet } from 'svelte';

	let {
		tag = 'div',
		class: className = '',
		children
	}: {
		tag?: keyof HTMLElementTagNameMap;
		class?: string;
		children?: Snippet;
	} = $props();
</script>

<svelte:element this={tag} class={['ds-container', className].filter(Boolean).join(' ')}>
	{#if children}
		{@render children()}
	{/if}
</svelte:element>
