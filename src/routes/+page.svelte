<script lang="ts">
	import Hero from '#blocks/hero/hero.svelte';
	import Features from '#blocks/features.svelte';
	import Testimonial from '#blocks/testimonial.svelte';
	import Pricing from '#blocks/pricing.svelte';
	import Faq from '#blocks/faq.svelte';
	import CallToAction from '#blocks/call-to-action.svelte';
	import Footer from '#blocks/footer.svelte';
	import FeatureTwo from '#lib/components/blocks/feature-two.svelte';
</script>

<div>
	<Hero />
	<!-- <Features /> -->
	<FeatureTwo />
	<Testimonial />
	<Pricing />
	<Faq />
	<CallToAction />
	<Footer />
</div>
