<script lang="ts">
	import { Button } from '#lib/components/ui/dusk/button/index.js';
	import { Card } from '#lib/components/ui/dusk/card/index.js';
	import ArrowRight from '@lucide/svelte/icons/arrow-right';
	import Badge from '../spell/badge/badge.svelte';

	type Props = {
		eyebrow?: string;
		title?: string;
		description?: string;
		ctaLabel?: string;
		ctaHref?: string;
	};

	let {
		eyebrow = 'Limited Time Offer',
		title = 'Start Building Today',
		description = 'Get 3 months free when you sign up for an annual plan. No credit card required to start.',
		ctaLabel = 'Claim Your Offer',
		ctaHref = '/'
	}: Props = $props();
</script>

<section class="@container bg-background py-24">
	<div class="mx-auto max-w-2xl px-6">
		<Card variant="outline" class="p-8 md:p-12">
			<Badge variant="emerald" class="mb-6 rounded-full px-3 text-xs">{eyebrow}</Badge>
			<h2 class="font-serif text-3xl font-medium text-balance md:text-4xl">{title}</h2>
			<p class="mt-4 max-w-md text-balance text-muted-foreground">
				{description}
			</p>
			<Button href={ctaHref} class="mt-8 gap-2">
				{ctaLabel}
				<ArrowRight class="size-4" />
			</Button>
		</Card>
	</div>
</section>
