<script lang="ts">
	import { goto } from '$app/navigation';
	import CardTopTemplate from '$lib/components/user/CardTopTemplate.svelte';
	import type { Auction, AuctionDummy } from '$lib/interfaces';
	import { scrollIntoView } from '$lib/utils/otherUtils';
	import { onMount } from 'svelte';

	let drawerContent: HTMLDivElement;
	onMount(() => {
		const _scroll = drawerContent.scrollTo;
		window.scrollTo = function () {
			_scroll.apply(drawerContent);
		};
	});

	const auction: AuctionDummy = {
		id: Math.floor(1000 + Math.random() * 9000),
		profile_name: 'Bored Ape Yacht Club',
		profile_desc: 'BoredApeYachtClub #8867',
		profile_pic: '/images/profile-images/Bored-Ape-Yach-Club.png',
		nft: '/images/hero-nft-1.png',
		crypto_price: 95.2,
		usd_price: 125029,
		liked: false
	};
	const auctions: AuctionDummy[] = [];
	// 	auction,
	// 	{
	// 		id: Math.floor(1000 + Math.random() * 9000),
	// 		profile_name: 'Strange Connections',
	// 		profile_desc: 'Strange Connections #1414',
	// 		profile_pic: '/images/profile-images/Strange-Connections.png',
	// 		nft: '/images/hero-nft-2.png',
	// 		crypto_price: 85.2,
	// 		usd_price: 122029,
	// 		liked: false
	// 	}
	// ];
</script>

<div
	bind:this={drawerContent}
	class="created-assets user-template"
	style="width: 100%;height: 100%"
>
	{#if auctions.length > 0}
		<div class="top">
			<div class="toolbar">
				<div class="search-container">
					<input type="text" name="search" placeholder="Search by items " />
					<img src="/icons/search-icon.svg" alt="" />
				</div>
				<select name="catergory">
					<option value="1">Catergory 1</option>
				</select>
			</div>
		</div>
		<div class="auctions-container">
			{#each auctions as auction}
				<div class="auction">
					<div class="auction-card">
						<div class="content">
							<CardTopTemplate
								profile_name={auction.profile_name}
								profile_desc={auction.profile_desc}
								profile_pic={auction.profile_pic}
								nft={auction.nft}
								liked={auction.liked}
								tokenId={1}
							/>
							<div class="auction-card-bottom">
								<div class="left">
									<span>Base Bid</span>
									<h4>{auction.crypto_price.toLocaleString()}<span>MATIC</span></h4>
									<span class="usd">${auction.usd_price.toLocaleString()}</span>
								</div>
								<div class="right">
									<span>Remaning Time</span>
									<h4>--h --m --s</h4>
									<p><span class="usd">0</span>bids placed</p>
								</div>
							</div>
							<div class="auction-btns">
								<button class="btn-primary auction-btn-place" style="width: 100%;">
									<span>Put on Auction</span>
								</button>
							</div>
						</div>
						<div class="bg " />
					</div>
				</div>
			{/each}
		</div>
	{:else}
		<div class="empty-disp">
			<h1>Create a digital asset to view here</h1>
			<span>
				Create a digital asset and archive it here. You can put it up for auction at any time
			</span>
			<button class="btn-primary short-button" on:click={() => goto('/explore')}>
				<span>Explore Auctions</span>
				<img src="/icons/arrow-forward-black.svg" alt="" />
			</button>
		</div>
	{/if}
</div>

<style>
	* {
		font-family: 'Darker Grotesque', sans-serif;
	}
	.created-assets .top {
		margin-bottom: 24px;
	}

	.created-assets .auctions-container {
		display: grid;
		grid-template-columns: repeat(auto-fill, 232px);
		grid-gap: 16px;
		z-index: 10;
		padding-bottom: 20px;
	}

	.created-assets .auctions-container .auction .auction-card {
		position: relative;
		z-index: 10;
	}

	.created-assets .auction-card {
		margin-bottom: 16px;
	}

	.created-assets .auction-card .content {
		padding: 13.35px 13.11px;
	}

	.created-assets .auction-card .content .auction-card-bottom .left > span,
	.created-assets .auction-card .content .auction-card-bottom .right > span,
	.created-assets .auction-card .content .auction-card-bottom .right p {
		font-size: 11.47px;
	}

	.created-assets .auction-card .content .auction-card-bottom .right p {
		font-weight: 500;
	}
	.created-assets .auction-card .content .auction-card-bottom .left h4,
	.created-assets .auction-card .content .auction-card-bottom .right h4 {
		font-size: 18.02px;
	}
	.created-assets .auction-card .content .auction-btns {
		align-items: normal;
	}
	.created-assets .auction-card .content .auction-btns .auction-btn-place {
		padding: 5px 20px;
		font-weight: 400;
		font-size: 14px;
	}

	.created-assets .auction-card .bg {
		background: #1d220d;
		border: 1.05854px solid #d3f85a;
		position: absolute;
		width: 230px;
		height: 389.46px;
		top: 12px;
		left: 12px;
		z-index: 1;
		display: none;
	}

	/* Hover */
	.created-assets .auction-card:hover .content .auction-btns .auction-btn-place {
		padding-top: 10px;
		padding-bottom: 10px;
		font-size: 15px;
	}

	.created-assets .auction-card:hover {
		margin-bottom: 0px;
	}

	.created-assets .auction-card:hover .bg {
		display: block;
		z-index: -1;
	}
</style>
