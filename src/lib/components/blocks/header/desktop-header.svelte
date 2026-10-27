<script lang="ts">
	import Logo from '#lib/components/web/Logo.svelte';
	import { Button } from '#lib/components/ui/dusk/button/index.js';
	import { navItems } from './nav.js';
</script>

<nav class="fixed top-0 z-20 hidden w-full bg-background lg:block" aria-label="Primary">
	<div class="mx-auto max-w-6xl px-6">
		<div class="flex h-14 items-center justify-between">
			<div class="flex items-center gap-12">
				<a href="/" aria-label="Home">
					<Logo class="size-5" />
				</a>

				<ul class="flex gap-8 text-sm">
					{#each navItems as item (item.name)}
						<li>
							<a
								href={item.href}
								class="block text-muted-foreground transition-colors duration-150 hover:text-accent-foreground"
							>
								{item.name}
							</a>
						</li>
					{/each}
				</ul>
			</div>

			<div class="flex items-center gap-3">
				<Button href="/" size="sm" variant="outline">Login</Button>
				<Button href="/" size="sm">Sign Up</Button>
			</div>
		</div>
	</div>
</nav>
