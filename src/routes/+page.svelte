<script>
	import HeroSection from '$lib/components/hero-section/HeroSection.svelte';
	import TopAuctionsSection from '$lib/components/top-auctions-section/TopAuctionsSection.svelte';
	import HowToSellSection from '$lib/components/how-to-sell-section/HowToSellSection.svelte';
	import TopSellersSection from '$lib/components/top-sellers-section/TopSellersSection.svelte';
	import NeverMissSection from '$lib/components/never-miss-section/NeverMissSection.svelte';
</script>

<svelte:head>
	<title>Dauction</title>
	<meta name="description" content="Dauction - Decentralized sealed-bidding auctions" />
</svelte:head>

<HeroSection />
<TopAuctionsSection />
<HowToSellSection />
<TopSellersSection />
<NeverMissSection />
