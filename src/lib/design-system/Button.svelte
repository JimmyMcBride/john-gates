<script lang="ts">
	import type { Snippet } from 'svelte';

	type ButtonVariant = 'primary' | 'secondary' | 'text';
	type ButtonSize = 'sm' | 'md' | 'lg';

	let {
		href,
		type = 'button',
		variant = 'primary',
		size = 'md',
		onclick,
		class: className = '',
		children
	}: {
		href?: string;
		type?: 'button' | 'submit' | 'reset';
		variant?: ButtonVariant;
		size?: ButtonSize;
		onclick?: () => void;
		class?: string;
		children?: Snippet;
	} = $props();
</script>

{#if href}
	<a
		{href}
		{onclick}
		class={['ds-button', `ds-button--${variant}`, `ds-button--${size}`, className]
			.filter(Boolean)
			.join(' ')}
	>
		{#if children}
			{@render children()}
		{/if}
	</a>
{:else}
	<button
		{type}
		{onclick}
		class={['ds-button', `ds-button--${variant}`, `ds-button--${size}`, className]
			.filter(Boolean)
			.join(' ')}
	>
		{#if children}
			{@render children()}
		{/if}
	</button>
{/if}
