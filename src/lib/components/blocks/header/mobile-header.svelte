<script lang="ts">
	import MenuIcon from '@lucide/svelte/icons/menu';
	import X from '@lucide/svelte/icons/x';
	import { Button } from '#lib/components/ui/dusk/button/index.js';
	import Logo from '#lib/components/web/Logo.svelte';
	import { watch } from 'runed';
	import { navItems } from './nav.js';
	import { fly } from 'svelte/transition';

	let menuOpen = $state(false);
	let menuButton: HTMLButtonElement | null = $state(null);
	const id = $props.id();
	const menuId = `${id}-mobile-menu`;

	watch(
		() => menuOpen,
		(isOpen) => {
			if (!isOpen) return;

			const mediaQuery = window.matchMedia('(max-width: 1023px)');
			const updateOverflow = () => {
				document.documentElement.classList.toggle('overflow-hidden', mediaQuery.matches);
			};

			updateOverflow();
			mediaQuery.addEventListener('change', updateOverflow);

			return () => {
				mediaQuery.removeEventListener('change', updateOverflow);
				document.documentElement.classList.remove('overflow-hidden');
			};
		}
	);

	function closeMenu() {
		menuOpen = false;
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.key !== 'Escape' || !menuOpen) return;

		closeMenu();
		menuButton?.focus();
	}
</script>

<svelte:window onkeydown={handleKeydown} />

<nav
	data-state={menuOpen ? 'active' : 'inactive'}
	class="fixed top-0 z-20 w-full bg-background data-[state=active]:bottom-0 data-[state=active]:overflow-y-auto data-[state=active]:overscroll-contain lg:hidden"
	aria-label="Primary"
>
	<div class="mx-auto max-w-6xl px-6">
		<div class="flex h-12 items-center justify-between">
			<a href="/" aria-label="Home">
				<Logo class="size-5" />
			</a>

			<Button
				bind:ref={menuButton}
				onclick={() => (menuOpen = !menuOpen)}
				aria-label={menuOpen ? 'Close menu' : 'Open menu'}
				aria-controls={menuId}
				aria-expanded={menuOpen}
				size="icon"
				variant="ghost"
			>
				{#if menuOpen}
					<X />
				{:else}
					<MenuIcon />
				{/if}
			</Button>
		</div>

		{#if menuOpen}
			<div id={menuId} class="flex flex-col pt-6 pb-6">
				<ul>
					{#each navItems as item, index}
						<li in:fly|global={{ y: -10, duration: 200, delay: index * 50 }}>
							<a
								href={item.href}
								onclick={closeMenu}
								class="block py-3 text-2xl font-medium text-foreground"
							>
								{item.name}
							</a>
						</li>
					{/each}
				</ul>

				<div
					in:fly|global={{ y: -10, duration: 200, delay: navItems.length * 50 }}
					class="mt-12 flex w-full flex-col gap-3 sm:flex-row"
				>
					<Button href="/" size="sm" variant="outline" onclick={closeMenu}>Login</Button>
					<Button href="/" size="sm" onclick={closeMenu}>Sign Up</Button>
				</div>
			</div>
		{/if}
	</div>
</nav>
