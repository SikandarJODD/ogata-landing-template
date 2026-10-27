<script lang="ts">
	import ArrowRight from '@lucide/svelte/icons/arrow-right';
	import Check from '@lucide/svelte/icons/check';
	import { AnimatePresence, motion, useReducedMotion } from 'motion-sv';

	type Visual = 'connect' | 'configure' | 'launch';

	type Step = {
		title: string;
		description: string;
		visual?: Visual;
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
				description: 'Bring your tools, data, and existing workflow together in one place.',
				visual: 'connect'
			},
			{
				title: 'Set up your process',
				description: 'Choose how your team works and adjust each step to fit your needs.',
				visual: 'configure'
			},
			{
				title: 'Launch and improve',
				description: 'Start working, follow progress, and refine the process as your team grows.',
				visual: 'launch'
			}
		]
	}: Props = $props();

	const id = $props.id();
	const visualId = `${id}-visual`;
	const reducedMotion = useReducedMotion();

	let activeIndex = $state(0);
	let animateSelection = $state(false);
	let activeStep = $derived(steps[activeIndex] ?? steps[0]);
	let motionEnabled = $derived(animateSelection && !reducedMotion.current);

	const activityBars = [
		{ height: 38, color: 'var(--chart-1)' },
		{ height: 54, color: 'var(--chart-2)' },
		{ height: 46, color: 'var(--chart-3)' },
		{ height: 70, color: 'var(--chart-4)' },
		{ height: 62, color: 'var(--chart-5)' },
		{ height: 84, color: 'var(--chart-2)' },
		{ height: 76, color: 'var(--chart-1)' }
	];

	function selectStep(index: number, event: MouseEvent) {
		if (index === activeIndex) return;

		animateSelection = event.detail > 0;
		activeIndex = index;
	}

	function enterFrom(x = 0, y = 0, scale = 1) {
		return motionEnabled
			? { opacity: 0, x, y, scale }
			: { opacity: 1, x: 0, y: 0, scale: 1 };
	}

	function enterTransition(delay = 0) {
		return motionEnabled
			? { duration: 0.2, delay, ease: [0.32, 0.72, 0, 1] as const }
			: { duration: 0 };
	}
</script>

<section class="bg-background py-16 md:py-24">
	<div class="mx-auto max-w-6xl px-6">
		<div class="max-w-2xl">
			<h2 class="text-3xl font-medium tracking-tight text-balance text-foreground md:text-4xl">
				{title}
			</h2>
			<p class="mt-4 max-w-xl text-muted-foreground">{description}</p>
		</div>

		<div class="mt-12 grid gap-8 lg:mt-16 lg:grid-cols-[0.72fr_1.28fr] lg:items-center lg:gap-12">
			<div class="flex flex-col gap-2" aria-label="How it works steps">
				{#each steps as step, index (step.title)}
					<button
						type="button"
						onclick={(event) => selectStep(index, event)}
						aria-controls={visualId}
						aria-pressed={activeIndex === index}
						data-active={activeIndex === index}
						class="group relative flex min-h-24 w-full gap-4 rounded-xl p-4 text-left data-[active=true]:bg-card data-[active=true]:shadow-sm focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-foreground"
					>
						<span
							class="flex size-8 shrink-0 items-center justify-center rounded-full bg-muted text-sm font-medium text-muted-foreground tabular-nums group-aria-pressed:bg-foreground group-aria-pressed:text-background"
						>
							{index + 1}
						</span>
						<span>
							<span class="block font-medium text-foreground">{step.title}</span>
							<span class="mt-1 block text-sm leading-6 text-muted-foreground">
								{step.description}
							</span>
						</span>
					</button>
				{/each}
			</div>

			<div
				id={visualId}
				class="relative flex min-h-96 items-center justify-center overflow-hidden rounded-3xl bg-muted/60 p-5 sm:p-8 lg:min-h-[30rem]"
			>
				<div class="grid w-full max-w-xl">
					<AnimatePresence initial={false} mode="sync">
						{#key activeIndex}
							<motion.div
								class="col-start-1 row-start-1 w-full self-center"
								initial={enterFrom(0, 8, 0.985)}
								animate={{ opacity: 1, x: 0, y: 0, scale: 1 }}
								exit={motionEnabled ? { opacity: 0, y: -4, scale: 0.99 } : { opacity: 1 }}
								transition={motionEnabled
									? { duration: 0.2, ease: [0.32, 0.72, 0, 1] }
									: { duration: 0 }}
							>
								{#if activeStep?.visual === 'configure'}
									{@render ConfigureVisual()}
								{:else if activeStep?.visual === 'launch'}
									{@render LaunchVisual()}
								{:else}
									{@render ConnectVisual()}
								{/if}
							</motion.div>
						{/key}
					</AnimatePresence>
				</div>
			</div>
		</div>
	</div>
</section>

{#snippet ConnectVisual()}
	<div aria-hidden="true" class="grid items-center gap-4 sm:grid-cols-[1fr_auto_1.15fr]">
		<div class="space-y-3">
			{#each [
				{ label: 'Customer data', color: 'var(--chart-1)' },
				{ label: 'Team tools', color: 'var(--chart-2)' },
				{ label: 'Product activity', color: 'var(--chart-3)' }
			] as source (source.label)}
				<div class="flex items-center gap-3 rounded-xl bg-background p-3 shadow-xs">
					<span class="size-2 rounded-full" style:background-color={source.color}></span>
					<span class="text-sm font-medium text-foreground">{source.label}</span>
				</div>
			{/each}
		</div>

		<div class="mx-auto hidden sm:block">
			<ArrowRight class="size-5 text-muted-foreground" />
		</div>

		<div class="rounded-2xl bg-background p-5 shadow-sm">
			<div class="flex items-center justify-between gap-4 pb-4">
				<span class="font-medium text-foreground">Shared workspace</span>
				<span class="flex items-center gap-1.5 text-xs text-muted-foreground">
					<span class="size-1.5 rounded-full bg-chart-3"></span>
					Connected
				</span>
			</div>
			<div class="space-y-4 pt-2">
				<div class="h-2 w-4/5 rounded-full bg-foreground/10"></div>
				<div class="h-2 w-full rounded-full bg-foreground/10"></div>
				<div class="h-2 w-3/5 rounded-full bg-foreground/10"></div>
				<div class="grid grid-cols-3 gap-2 pt-3">
					<div class="h-16 rounded-lg bg-chart-1/12"></div>
					<div class="h-16 rounded-lg bg-chart-2/12"></div>
					<div class="h-16 rounded-lg bg-chart-3/12"></div>
				</div>
			</div>
		</div>
	</div>
{/snippet}

{#snippet ConfigureVisual()}
	<div aria-hidden="true" class="rounded-2xl bg-background p-4 shadow-sm sm:p-6">
		<div class="flex items-center gap-2 pb-4">
			<span class="size-2 rounded-full bg-chart-1"></span>
			<span class="size-2 rounded-full bg-chart-2"></span>
			<span class="size-2 rounded-full bg-chart-3"></span>
			<span class="ml-2 text-sm font-medium text-muted-foreground">Product workflow</span>
		</div>

		<div class="grid gap-3 pt-2 sm:grid-cols-3">
			{#each [
				{ title: 'Collect', description: 'Bring new work into one place.', color: 'var(--chart-1)' },
				{ title: 'Review', description: 'Keep decisions clear and visible.', color: 'var(--chart-2)' },
				{ title: 'Deliver', description: 'Move approved work forward.', color: 'var(--chart-3)' }
			] as stage, index (stage.title)}
				<motion.div
					class="relative rounded-xl bg-card p-4 shadow-xs"
					initial={enterFrom(0, 10, 0.985)}
					animate={{ opacity: 1, x: 0, y: 0, scale: 1 }}
					transition={enterTransition(index * 0.04)}
				>
					<span class="text-xs font-medium text-muted-foreground tabular-nums">{index + 1}</span>
					<p class="mt-5 font-medium text-foreground">{stage.title}</p>
					<p class="mt-2 text-sm leading-6 text-muted-foreground">{stage.description}</p>
					<div class="mt-6 h-1.5 overflow-hidden rounded-full bg-muted">
						<motion.div
							class="h-full w-full origin-left rounded-full"
							style={{ backgroundColor: stage.color }}
							initial={motionEnabled ? { scaleX: 0 } : { scaleX: 1 }}
							animate={{ scaleX: (72 - index * 14) / 100 }}
							transition={enterTransition(0.06 + index * 0.04)}
						/>
					</div>
				</motion.div>
			{/each}
		</div>
	</div>
{/snippet}

{#snippet LaunchVisual()}
	<div aria-hidden="true" class="grid gap-4 sm:grid-cols-[1.15fr_0.85fr]">
		<motion.div
			class="rounded-2xl bg-background p-5 shadow-sm"
			initial={enterFrom(-10, 0, 0.985)}
			animate={{ opacity: 1, x: 0, y: 0, scale: 1 }}
			transition={enterTransition()}
		>
			<div class="flex items-center justify-between pb-4">
				<span class="font-medium text-foreground">Launch progress</span>
				<span class="text-sm text-muted-foreground tabular-nums">3 of 3</span>
			</div>
			<div class="space-y-4 pt-2">
				{#each ['Workspace connected', 'Workflow configured', 'Team ready'] as item, index (item)}
					<motion.div
						class="flex items-center gap-3"
						initial={enterFrom(-8)}
						animate={{ opacity: 1, x: 0, y: 0, scale: 1 }}
						transition={enterTransition(0.035 + index * 0.035)}
					>
						<motion.span
							class="flex size-6 items-center justify-center rounded-full bg-chart-3 text-background"
							initial={motionEnabled ? { scale: 0.92 } : { scale: 1 }}
							animate={{ scale: 1 }}
							transition={enterTransition(0.06 + index * 0.035)}
						>
							<Check class="size-3.5" strokeWidth={2.5} />
						</motion.span>
						<span class="text-sm font-medium text-foreground">{item}</span>
					</motion.div>
				{/each}
			</div>
		</motion.div>

		<motion.div
			class="rounded-2xl bg-background p-5 shadow-sm"
			initial={enterFrom(10, 0, 0.985)}
			animate={{ opacity: 1, x: 0, y: 0, scale: 1 }}
			transition={enterTransition(0.04)}
		>
			<p class="font-medium text-foreground">Activity</p>
			<div class="mt-8 flex h-32 items-end gap-2">
				{#each activityBars as bar, index (`${bar.height}-${index}`)}
					<div class="flex h-full flex-1 items-end overflow-hidden rounded-sm bg-muted">
						<motion.div
							class="h-full w-full origin-bottom rounded-sm"
							style={{ backgroundColor: bar.color }}
							initial={motionEnabled ? { scaleY: 0 } : { scaleY: bar.height / 100 }}
							animate={{ scaleY: bar.height / 100 }}
							transition={enterTransition(0.04 + index * 0.035)}
						/>
					</div>
				{/each}
			</div>
			<p class="mt-4 text-sm text-muted-foreground">Progress stays visible as work moves forward.</p>
		</motion.div>
	</div>
{/snippet}
