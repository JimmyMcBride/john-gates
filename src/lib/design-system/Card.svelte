<script lang="ts">
	import type { Snippet } from 'svelte';

	let {
		class: className = '',
		children,
		icon
	}: {
		class?: string;
		children?: Snippet;
		icon?: Snippet;
	} = $props();
</script>

<article class={['group', 'ds-card', className].filter(Boolean).join(' ')}>
	{#if icon}
		<div class="ds-card__icon">
			{@render icon()}
		</div>
	{/if}

	{#if children}
		{@render children()}
	{/if}
</article>
