<script lang="ts">
	import { Card } from '#lib/components/ui/dusk/card/index.js';

	type UseCase = {
		title: string;
		description: string;
		outcomes: string[];
	};

	type Props = {
		title?: string;
		description?: string;
		items?: UseCase[];
	};

	let {
		title = 'Built for the way your team works',
		description = 'Flexible enough for different teams, without adding complexity to the work they already do.',
		items = [
			{
				title: 'Developers',
				description: 'Keep technical work focused and connected from planning through delivery.',
				outcomes: ['Reduce repetitive setup', 'Keep context close to the work']
			},
			{
				title: 'Startups',
				description: 'Move quickly with a clear process that can grow alongside the company.',
				outcomes: ['Launch with less overhead', 'Adapt as priorities change']
			},
			{
				title: 'Product teams',
				description: 'Bring planning, feedback, and delivery into a shared view for the whole team.',
				outcomes: ['Align work around priorities', 'Make progress easier to follow']
			},
			{
				title: 'Agencies',
				description: 'Manage client work consistently while keeping every project easy to understand.',
				outcomes: ['Standardize repeatable workflows', 'Give clients clearer visibility']
			}
		]
	}: Props = $props();
</script>

<section class="bg-background py-16 md:py-24">
	<div class="mx-auto max-w-6xl px-6">
		<div class="max-w-2xl">
			<h2 class="text-3xl font-medium tracking-tight text-balance text-foreground md:text-4xl">
				{title}
			</h2>
			<p class="mt-4 max-w-xl text-muted-foreground">{description}</p>
		</div>

		<div class="mt-12 grid gap-3 md:grid-cols-2 lg:mt-16">
			{#each items as item (item.title)}
				<Card variant="outline" class="p-6 md:p-8">
					<h3 class="text-lg font-semibold text-foreground">{item.title}</h3>
					<p class="mt-3 max-w-lg text-muted-foreground">{item.description}</p>

					<ul class="mt-6 space-y-3 text-sm text-foreground">
						{#each item.outcomes as outcome (outcome)}
							<li class="flex items-start gap-3">
								<span aria-hidden="true" class="mt-2 size-1.5 shrink-0 rounded-full bg-primary"></span>
								<span>{outcome}</span>
							</li>
						{/each}
					</ul>
				</Card>
			{/each}
		</div>
	</div>
</section>
