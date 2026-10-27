<script lang="ts">
	import {
		Beacon,
		Bolt,
		Hulu,
		Spotify,
		SupabaseFull,
		VercelFull
	} from '#lib/components/svgs/index.ts';
</script>

<section class="bg-background">
	<div class="relative m-auto max-w-7xl px-6 **:fill-foreground">
		<div
			class="flex flex-wrap items-center justify-center gap-x-11 gap-y-8 py-8 lg:justify-between lg:py-12"
		>
			<Hulu class="h-5 w-15" />
			<VercelFull class="h-6 w-25" />
			<Bolt class="h-6 w-14.5" />
			<SupabaseFull class="h-7 w-auto" />
			<Beacon class="h-6.5 w-23" />
			<Spotify class="h-7 w-22.5" />
		</div>
	</div>
</section>
