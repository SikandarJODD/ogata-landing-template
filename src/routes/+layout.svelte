<script lang="ts">
	import './layout.css';
	import favicon from '#lib/assets/favicon.svg';
	import type { LayoutProps } from './$types';
	import { ModeWatcher, toggleMode } from 'mode-watcher';
	import { activeElement, PressedKeys } from 'runed';

	let { children }: LayoutProps = $props();
	let keys = new PressedKeys();
	keys.onKeys(['d'], () => {
		if (
			activeElement.current?.localName === 'input' ||
			activeElement.current?.localName === 'textarea'
		) {
			return;
		}
		toggleMode();
	});
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<ModeWatcher defaultMode="system" />
{@render children()}
