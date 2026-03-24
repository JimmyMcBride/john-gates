<script lang="ts">
	import { onMount } from 'svelte';
	import { Button, Card, Container, Field, SectionHeader } from '$lib';

	type NavLink = {
		label: string;
		href: string;
		highlight?: boolean;
	};

	type ValueCard = {
		icon: string;
		title: string;
		copy: string;
	};

	type FeaturedWork = {
		title: string;
		category: string;
		image: string;
		alt: string;
	};

	type ProcessStep = {
		title: string;
		copy: string;
	};

	const navLinks: NavLink[] = [
		{ label: 'Portfolio', href: '#portfolio', highlight: true },
		{ label: 'Process', href: '#process' },
		{ label: 'About', href: '#about' },
		{ label: 'Contact', href: '#contact' }
	];

	const valueCards: ValueCard[] = [
		{
			icon: 'architecture',
			title: 'Built with care',
			copy:
				'Clean lines, tight fits, and hardware that feels right. The small things are the job.'
		},
		{
			icon: 'handshake',
			title: 'Easy to work with',
			copy:
				'You get straight answers, practical suggestions, and a builder who listens before he starts cutting.'
		},
		{
			icon: 'verified',
			title: 'Years on the tools',
			copy:
				'Fifty years in construction changes how you build. It has to look good, but it also has to hold up.'
		}
	];

	const featuredPrimary: FeaturedWork = {
		title: 'The Oak Minimalist',
		category: 'Modern',
		image:
			'https://lh3.googleusercontent.com/aida-public/AB6AXuApU0_8_GgW2z4-uWYXfx4TkzVdWVWI_wF8vFUaOlHyxgECDBNt8_tsjvscHFOGoWHEBliqI5sVo-mRmZCCR3EVGrZTgy5kZFfFNHaGAXBKXyrjqUBo2TfYohtLNzs6RGE8mswre9LkxiaO4A_r_rWXeK3a32iwOE-jv67mlJcbR-gBme_Hk_TlmZpuwNbrM6tRUSORRhFkOgxLNFkTXRG4_MMlff8LhSq-ATaVz1UhqjGf5nIhxgwXwJuTPbV_7yLexDVL1yUIlUZ5',
		alt: 'Modern minimalist kitchen with light oak cabinetry and floating shelves, clean lines and natural morning light'
	};

	const featuredSecondary: FeaturedWork[] = [
		{
			title: 'Cherry Wood Heritage',
			category: 'Traditional',
			image:
				'https://lh3.googleusercontent.com/aida-public/AB6AXuDKPcZeCxrZ3ntXo1fc_qGQTAHBvLUq_54BG5RiRti_VLquCZaZCn5x6B0seqDrZ7QQjEikd2i0sKfirfCbVIi8hN8lcGWqel7k87BEmlpQLF0NOpqI7owxj36EAdpgoh5DGJ1wEzcQ9N8rwJb9I-zvWc7nlqJVVt6LazZfaqyOBvFXwACkrgeivjtiKWB1PGmMNHXCK9qzLi_USjE4RI_cYsxYHL4pbfb4iWjqMjDb9nBMxG2AAhHW2vxSPkuRlzMkkG_NuLOth44z',
			alt: 'Traditional kitchen detail showing dark stained cherry wood cabinets with ornate crown molding and brass hardware'
		},
		{
			title: 'Reclaimed Pine Vanity',
			category: 'Rustic',
			image:
				'https://lh3.googleusercontent.com/aida-public/AB6AXuBfvHO03eHmyxmzE3USvEP7uxioRMHsXdEhpE65WvsiOn7GAV7vsAx8j1jdu8oPBTA2peu19mw3dXjjEmhIzCocBMejh97hOjE2VehhvnvbEDEKE369SsIGtNn2r9b2WLJ-KMXeMBAuuZpcfV122BLTg6rSA1BzMWOwTgkSe51y8fxdqy4c_ha99HZ_XMH2xAg0g5c5AqtOGyb_NqAa8PSW6LBLAic3Q5RTwAzkC0_mGc6-ItYaDpMdiyWMrd3x9YL1PsrhhcS0epD6',
			alt: 'Rustic hand-crafted vanity in a cabin setting made from reclaimed timber with visible wood grain and texture'
		}
	];

	const processSteps: ProcessStep[] = [
		{
			title: 'Talk it through',
			copy:
				'We go over the room, the way you use it, and what is not working now. Then we figure out a better plan.'
		},
		{
			title: 'Build it right',
			copy:
				'Each piece is built with solid materials, careful measurements, and the kind of patience this work takes.'
		},
		{
			title: 'Finish on site',
			copy:
				'Final fit and installation happen with the same care as the shop work, so everything lands the way it should.'
		}
	];

	const footerLinks: NavLink[] = [
		{ label: 'Privacy Policy', href: '#' },
		{ label: 'Terms of Service', href: '#' },
		{ label: 'Instagram', href: '#' },
		{ label: 'LinkedIn', href: '#' }
	];

	const currentYear = new Date().getFullYear();
	const heroFrameCount = 121;
	const heroFrames = Array.from({ length: heroFrameCount }, (_, index) => {
		const frameNumber = String(index + 1).padStart(3, '0');
		return `/hero-frames/frame-${frameNumber}.jpg`;
	});

	let mobileNavOpen = $state(false);
	let heroSection: HTMLElement | null = null;
	let currentHeroFrame = $state(0);

	onMount(() => {
		if (!heroSection) {
			return;
		}

		const motionQuery = window.matchMedia('(prefers-reduced-motion: reduce)');
		let prefersReducedMotion = motionQuery.matches;
		let frameId = 0;
		let destroyed = false;
		let preloadTimer = 0;

		const syncFrameToScroll = () => {
			if (!heroSection || prefersReducedMotion) {
				return;
			}

			const rect = heroSection.getBoundingClientRect();
			const scrollableDistance = heroSection.offsetHeight - window.innerHeight;

			if (scrollableDistance <= 0) {
				currentHeroFrame = 0;
				return;
			}

			const heroIsVisible = rect.bottom > 0 && rect.top < window.innerHeight;

			if (!heroIsVisible) {
				return;
			}

			const sectionProgress = Math.min(
				Math.max(-rect.top / scrollableDistance, 0),
				1
			);
			const nextFrame = Math.min(
				Math.round(sectionProgress * (heroFrameCount - 1)),
				heroFrameCount - 1
			);

			if (nextFrame !== currentHeroFrame) {
				currentHeroFrame = nextFrame;
			}
		};

		const animationLoop = () => {
			if (destroyed) {
				return;
			}

			syncFrameToScroll();
			frameId = window.requestAnimationFrame(animationLoop);
		};

		const handleMotionPreference = (event: MediaQueryListEvent) => {
			prefersReducedMotion = event.matches;

			if (prefersReducedMotion) {
				currentHeroFrame = 0;
			}
		};

		motionQuery.addEventListener('change', handleMotionPreference);
		frameId = window.requestAnimationFrame(animationLoop);
		preloadTimer = window.setTimeout(() => {
			for (const frame of heroFrames) {
				const image = new Image();
				image.src = frame;
			}
		}, 0);

		return () => {
			destroyed = true;
			window.cancelAnimationFrame(frameId);
			window.clearTimeout(preloadTimer);
			motionQuery.removeEventListener('change', handleMotionPreference);
		};
	});
</script>

<svelte:head>
	<title>John Gates | Master Cabinetry & Construction</title>
	<meta
		name="description"
		content="Custom cabinetry and finish work built with care, experience, and attention to the details that matter."
	/>
</svelte:head>

<div class="overflow-x-clip">
	<header
		class="fixed inset-x-0 top-0 z-50 border-b border-border-subtle bg-surface-page/85 supports-[backdrop-filter]:bg-surface-page/70 supports-[backdrop-filter]:backdrop-blur-xl"
	>
		<Container tag="nav" class="flex h-[var(--ds-layout-nav-height)] items-center justify-between font-display tracking-tight">
			<a href="#top" class="text-2xl font-semibold text-text-accent">John Gates</a>

			<div class="hidden items-center gap-8 md:flex">
				{#each navLinks as link}
					<a
						href={link.href}
						class={`pb-1 text-base transition-all ${
							link.highlight
								? 'border-b-2 border-action-primary font-bold text-text-accent'
								: 'font-medium text-text-muted hover:text-text-accent'
						}`}
					>
						{link.label}
					</a>
				{/each}

				<Button href="#contact" size="sm">
					Get a Quote
				</Button>
			</div>

			<button
				type="button"
				class="inline-flex items-center justify-center text-text-accent md:hidden"
				aria-label={mobileNavOpen ? 'Close navigation' : 'Open navigation'}
				aria-expanded={mobileNavOpen}
				onclick={() => (mobileNavOpen = !mobileNavOpen)}
			>
				<span class="material-symbols-outlined text-[2rem]">
					{mobileNavOpen ? 'close' : 'menu'}
				</span>
			</button>
		</Container>

		{#if mobileNavOpen}
			<div class="border-t border-border-subtle bg-surface-page/95 md:hidden">
				<Container class="flex flex-col gap-5 py-6">
					{#each navLinks as link}
						<a
							href={link.href}
							class="font-display text-xl text-text-accent"
							onclick={() => (mobileNavOpen = false)}
						>
							{link.label}
						</a>
					{/each}

					<Button href="#contact" size="sm" class="w-fit" onclick={() => (mobileNavOpen = false)}>
						Get a Quote
					</Button>
				</Container>
			</div>
		{/if}
	</header>

	<main id="top">
		<section bind:this={heroSection} class="relative h-[220vh]">
			<div class="sticky top-0 flex min-h-screen items-center overflow-hidden pt-20">
				<div class="absolute inset-0">
					<img
						class="h-full w-full object-cover"
						src={heroFrames[currentHeroFrame]}
						alt=""
						aria-hidden="true"
						fetchpriority="high"
						decoding="async"
					/>
					<div class="absolute inset-0 bg-gradient-to-r from-surface-page/95 via-surface-page/55 to-transparent"></div>
				</div>

				<Container class="relative z-10">
					<div class="max-w-2xl">
						<span class="ds-eyebrow mb-6 inline-block">Established 1974</span>
						<h1 class="ds-display-copy max-w-[14ch] font-display text-text-accent italic">
							Custom cabinetry built the old-fashioned way.
						</h1>
						<p class="ds-intro-copy mt-8 max-w-xl text-text-muted">
							Kitchens, built-ins, vanities, and finish work made with care, experience,
							and a close eye for fit.
						</p>
						<div class="mt-10 flex flex-col gap-4 sm:flex-row">
							<Button href="#contact" size="lg" class="group shadow-button">
								Talk About Your Project
								<span class="material-symbols-outlined transition-transform group-hover:translate-x-1">
									arrow_forward
								</span>
							</Button>
							<Button href="#portfolio" variant="secondary" size="lg">
								See the Work
							</Button>
						</div>
					</div>
				</Container>
			</div>
		</section>

		<section id="about" class="bg-surface-section py-[var(--ds-layout-section-y)]">
			<Container class="grid grid-cols-1 items-center gap-16 md:grid-cols-2">
				<div class="relative">
					<div class="aspect-[4/5] overflow-hidden rounded-lg shadow-[var(--ds-shadow-card-hover)]">
							<img
								class="h-full w-full object-cover"
								src="https://lh3.googleusercontent.com/aida-public/AB6AXuAOHvPav_rdd3G2jS0RCN3nhD5dzZV74cAL5c6QdLXycz896KnHOIXrDVNLWX4omEnu4m_nsKvueQY9DKXt_QT2TJV7OsgDg--ZHCyM9aoPmPxrSJ5PaRbHp3QSm0DOFYdXSY5w4zapg5QPw9-HNP3XV9QM-FPN3lcA-egYuvnK8PAxoX571O44xjVTUEGpaeaevxlFxRiKVlWhzp9V4BUnhUMJ0tmCiTrDSxfrNwVfTnH9rjNtkf3XBuWqKcFNa1-nNRr8cRhd7bOR"
								alt="Close-up of a craftsman's hands meticulously marking wood with a traditional pencil and square in a sunlit workshop"
								loading="lazy"
								decoding="async"
							/>
					</div>
					<div
						class="absolute -bottom-8 -right-2 hidden max-w-[240px] rounded-lg bg-action-tertiary p-8 shadow-xl lg:block"
					>
						<p class="mb-2 font-display text-4xl italic text-text-on-tertiary">50 Years</p>
						<p class="text-sm uppercase tracking-[0.18em] text-text-on-tertiary/80">
							of master construction excellence
						</p>
					</div>
				</div>

				<div class="space-y-8">
					<SectionHeader
						title="A lifetime in the trade"
						intro="John Gates has spent 50 years in construction, with the last two decades focused on custom cabinetry. That kind of time in the trade teaches you what lasts and what does not."
						class="max-w-xl"
					/>
					<p class="text-lg leading-relaxed text-text-muted">
						He works closely with homeowners from the first conversation through the final
						install, keeping the process clear and the work honest.
					</p>
					<div class="pt-4">
						<div class="flex items-center gap-4 font-display text-xl italic text-text-accent">
							<span class="h-px w-12 bg-border-default"></span>
							Good work should feel solid the day it goes in and ten years later.
						</div>
					</div>
				</div>
			</Container>
		</section>

		<section class="bg-surface-muted py-[var(--ds-layout-section-y)]">
			<Container>
				<SectionHeader title="Why people hire John" align="center" class="mb-16" />
				<div class="grid grid-cols-1 gap-8 md:grid-cols-3">
				{#each valueCards as card}
					<Card>
						{#snippet icon()}
							<span class="material-symbols-outlined text-3xl">{card.icon}</span>
						{/snippet}
						<h3 class="mb-4 font-display text-2xl text-text-accent">{card.title}</h3>
						<p class="leading-relaxed text-text-muted">{card.copy}</p>
					</Card>
				{/each}
				</div>
			</Container>
		</section>

		<section id="portfolio" class="bg-surface-section py-[var(--ds-layout-section-y)]">
			<Container class="mb-16 flex flex-col items-start justify-between gap-6 md:flex-row md:items-end">
				<SectionHeader
					title="Featured Works"
					intro="A few recent spaces, each built around the way the home is actually used."
					class="max-w-xl"
				/>

				<Button href="#contact" variant="text" size="sm">
					See More Projects
				</Button>
			</Container>

			<Container class="grid grid-cols-1 gap-6 md:h-[800px] md:grid-cols-12">
				<article class="group relative overflow-hidden rounded-lg md:col-span-8">
					<img
						class="h-[26rem] w-full object-cover transition-transform duration-700 group-hover:scale-105 md:h-full"
						src={featuredPrimary.image}
						alt={featuredPrimary.alt}
						loading="lazy"
						decoding="async"
					/>
					<div class="absolute inset-x-0 bottom-0 bg-gradient-to-t from-black/65 to-transparent p-8">
						<span class="mb-2 inline-block rounded bg-action-accent px-3 py-1 text-[10px] uppercase tracking-[0.24em] text-white">
							{featuredPrimary.category}
						</span>
						<h3 class="font-display text-2xl italic text-white">{featuredPrimary.title}</h3>
					</div>
				</article>

				<div class="space-y-6 md:col-span-4 md:flex md:flex-col">
					{#each featuredSecondary as project}
						<article class="group relative h-[18rem] overflow-hidden rounded-lg md:h-auto md:flex-1">
							<img
								class="h-full w-full object-cover transition-transform duration-700 group-hover:scale-105"
								src={project.image}
								alt={project.alt}
								loading="lazy"
								decoding="async"
							/>
							<div class="absolute inset-x-0 bottom-0 bg-gradient-to-t from-black/65 to-transparent p-6">
								<span class="mb-2 inline-block rounded bg-action-accent px-3 py-1 text-[10px] uppercase tracking-[0.24em] text-white">
									{project.category}
								</span>
								<h3 class="font-display text-xl italic text-white">{project.title}</h3>
							</div>
						</article>
					{/each}
				</div>
			</Container>
		</section>

		<section id="process" class="bg-surface-accent py-[var(--ds-layout-section-y)]">
			<Container>
				<SectionHeader
					eyebrow="How We Work"
					title="What the process looks like"
					align="center"
					class="mb-20"
				/>

				<div class="relative grid grid-cols-1 gap-8 md:grid-cols-3 md:gap-0">
					<div
						class="absolute left-0 top-6 hidden h-px w-full bg-border-default/40 md:block"
						aria-hidden="true"
					></div>

					{#each processSteps as step, index}
						<article class="relative z-10 flex flex-col items-center p-8 text-center">
							<div
								class="mb-8 flex h-12 w-12 items-center justify-center rounded-full bg-action-primary font-display text-xl text-text-on-action"
							>
								{index + 1}
							</div>
							<h3 class="mb-4 font-display text-2xl italic text-text-accent">{step.title}</h3>
							<p class="max-w-sm text-text-muted">{step.copy}</p>
						</article>
					{/each}
				</div>
			</Container>
		</section>

		<section id="contact" class="bg-surface-section py-[var(--ds-layout-section-y)]">
			<div class="mx-auto max-w-4xl px-6 text-center sm:px-8 lg:px-16">
				<div class="mb-8 inline-flex items-center justify-center rounded-full bg-action-tertiary-soft/10 p-4">
					<span class="material-symbols-outlined text-4xl text-action-tertiary">edit_note</span>
				</div>
				<SectionHeader
					title="Ready to get started?"
					intro="If you are planning a kitchen, built-in, vanity, or another custom piece, reach out and tell us what you have in mind."
					align="center"
					class="mx-auto max-w-3xl"
				/>

				<form
					class="mx-auto mt-12 max-w-lg space-y-6 text-left"
					onsubmit={(event) => event.preventDefault()}
				>
					<div>
						<label class="sr-only" for="name">Your Name</label>
						<Field
							id="name"
							name="name"
							placeholder="Your Name"
						/>
					</div>
					<div>
						<label class="sr-only" for="email">Email Address</label>
						<Field
							id="email"
							name="email"
							type="email"
							placeholder="Email Address"
						/>
					</div>
					<div>
						<label class="sr-only" for="project">Tell us about your project</label>
						<Field
							as="textarea"
							id="project"
							name="project"
							rows={3}
							placeholder="Tell me a little about the job"
						/>
					</div>
					<Button type="submit" size="lg" class="mt-8 w-full text-sm uppercase tracking-[0.24em]">
						Request a Quote
					</Button>
				</form>
			</div>
		</section>
	</main>

	<footer class="bg-surface-accent">
		<Container
			class="flex flex-col items-center justify-between gap-8 py-12 text-center text-sm uppercase tracking-[0.2em] text-text-accent md:flex-row md:text-left"
		>
			<div>
				<p class="font-display text-lg normal-case tracking-normal text-text-accent">
					John Gates Master Cabinetry
				</p>
				<p class="mt-2 text-[10px] tracking-[0.24em] text-text-accent/75">
					&copy; {currentYear} John Gates Master Cabinetry. Built with care since 1974.
				</p>
			</div>

			<div class="flex flex-wrap items-center justify-center gap-8">
				{#each footerLinks as link}
					<a href={link.href} class="text-text-muted hover:text-text-accent hover:opacity-100">
						{link.label}
					</a>
				{/each}
			</div>
		</Container>
	</footer>
</div>
