<script lang="ts">
	type HeaderAlign = 'left' | 'center';

	let {
		eyebrow,
		title,
		intro,
		align = 'left',
		class: className = ''
	}: {
		eyebrow?: string;
		title: string;
		intro?: string;
		align?: HeaderAlign;
		class?: string;
	} = $props();
</script>

<div class={['ds-section-header', `ds-section-header--${align}`, className].filter(Boolean).join(' ')}>
	{#if eyebrow}
		<span class="ds-eyebrow">{eyebrow}</span>
	{/if}

	<h2 class="ds-section-title">{title}</h2>

	{#if intro}
		<p class="ds-section-intro">{intro}</p>
	{/if}
</div>
