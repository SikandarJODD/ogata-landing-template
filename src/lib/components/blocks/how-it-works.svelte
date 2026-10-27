<script lang="ts">
	type Step = {
		title: string;
		description: string;
	};

	type Props = {
		title?: string;
		description?: string;
		steps?: Step[];
	};

	let {
		title = 'How it works',
		description = 'A simple process that helps your team move from setup to results without unnecessary complexity.',
		steps = [
			{
				title: 'Connect your workspace',
				description: 'Bring your tools, data, and existing workflow together in one place.'
			},
			{
				title: 'Set up your process',
				description: 'Choose how your team works and adjust each step to fit your needs.'
			},
			{
				title: 'Launch and improve',
				description: 'Start working, follow progress, and refine the process as your team grows.'
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

		<ol class="mt-12 grid gap-8 md:grid-cols-3 md:gap-6 lg:mt-16">
			{#each steps as step, index (step.title)}
				<li class="border-t border-dashed pt-6">
					<span class="text-sm font-medium text-muted-foreground tabular-nums">{index + 1}</span>
					<h3 class="mt-4 text-lg font-semibold text-foreground">{step.title}</h3>
					<p class="mt-2 max-w-sm text-muted-foreground">{step.description}</p>
				</li>
			{/each}
		</ol>
	</div>
</section>
