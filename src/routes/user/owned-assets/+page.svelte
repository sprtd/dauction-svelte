<script lang="ts">
	import { goto } from '$app/navigation';
	import CardTopTemplate from '$lib/components/user/CardTopTemplate.svelte';
	import type { Auction, AuctionDummy } from '$lib/interfaces';
	import { USER_NFTS, nftToAuction } from '$lib/stores/main';
	import { NFT_CONTRACT_ADDRESS_ON_MUMBAI, RANDOM_PROFILE } from '$lib/utils/constants';
	import { scrollIntoView } from '$lib/utils/otherUtils';
	import { onMount } from 'svelte';

	let drawerContent: HTMLDivElement;
	onMount(() => {
		const _scroll = drawerContent.scrollTo;
		window.scrollTo = function () {
			_scroll.apply(drawerContent);
		};
	});

	const putOnAuction = async (nft: any) => {
		await nftToAuction.set({
			contractAddress: NFT_CONTRACT_ADDRESS_ON_MUMBAI,
			tokenId: nft.tokenId
		});
		goto('/user/create-auction');
		return;
	};

	/*	const auction: AuctionDummy = {
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

	*/

	const randomProfilePic = RANDOM_PROFILE[Math.floor(Math.random() * RANDOM_PROFILE.length)];
</script>

<div bind:this={drawerContent} class="owned-assets user-template" style="width: 100%;height: 100%">
	{#if $USER_NFTS.length > 0}
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
			{#each $USER_NFTS as nft}
				<div class="auction">
					<div class="auction-card">
						<div class="content">
							<CardTopTemplate
								profile_name={'auction.profile_name'}
								profile_desc={'auction.profile_desc'}
								profile_pic={randomProfilePic}
								nft={nft.image}
								liked={nft.liked}
								showLike={false}
								tokenId={nft.tokenId}
							/>
							<div class="auction-card-bottom">
								<!-- <div class="left">
									<span>Base Bid</span>
									<h4>{auction.crypto_price.toLocaleString()}<span>MATIC</span></h4>
									<span class="usd">${auction.usd_price.toLocaleString()}</span>
								</div> -->
								<div class="left">
									<!-- <span>Token Id</span> -->
									<h4>{`Token Id [${nft.tokenId}]`}</h4>
									<!-- <span class="usd">${auction.usd_price.toLocaleString()}</span> -->
								</div>
								<!-- <div class="right">
									<span style="color:var(--primary) !important">Your Winning Bid</span>
									<h4>
										{(
											auction.crypto_price + Math.floor(10 + Math.random() * 1000)
										).toLocaleString()}
										<span>MATIC</span>
									</h4>
									<p>
										${(
											auction.usd_price + Math.floor(10 + Math.random() * 300000)
										).toLocaleString()}
									</p>
								</div> -->
							</div>
							<div class="auction-btns">
								<button
									class="btn-outline-primary auction-btn-explore"
									on:click={() => putOnAuction(nft)}
								>
									<span>Put on Auction</span>
								</button>
								<button class="btn-outline-primary auction-btn-explore" style="width: 38%;">
									<span>View</span>
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
			<h1>No Assets Owned</h1>
			<span>
				You currently have no assets owned. Explore assets on auction and place a bid to own assets
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
	.owned-assets .top {
		margin-bottom: 24px;
	}

	.owned-assets .auctions-container {
		display: grid;
		grid-template-columns: repeat(auto-fill, 232px);
		grid-gap: 16px;
		z-index: 10;
		padding-bottom: 20px;
	}

	.owned-assets .auctions-container .auction .auction-card {
		position: relative;
		z-index: 10;
	}

	.owned-assets .auction-card {
		margin-bottom: 16px;
	}

	.owned-assets .auction-card .content {
		padding: 13.35px 13.11px;
	}

	.owned-assets .auction-card .content .auction-card-bottom .left > span,
	.owned-assets .auction-card .content .auction-card-bottom .right > span,
	.owned-assets .auction-card .content .auction-card-bottom .right p {
		font-size: 11.47px;
	}

	.owned-assets .auction-card .content .auction-card-bottom .right p {
		font-weight: 500;
	}
	.owned-assets .auction-card .content .auction-card-bottom .left h4,
	.owned-assets .auction-card .content .auction-card-bottom .right h4 {
		font-size: 15.02px;
		font-weight: 700;
	}
	.owned-assets .auction-card .content .auction-btns {
		align-items: normal;
	}

	.owned-assets .auction-card .content .auction-card-bottom .right h4 span {
		font-weight: 800;
		font-size: 14px;
		line-height: 118.1%;
		margin-left: 2px;
		color: var(--dark-tint-2);
	}

	.owned-assets .auction-card .content .auction-btns .auction-btn-explore {
		padding: 5px 20px;
		font-weight: 400;
		font-size: 14px;
	}

	.owned-assets .auction-card .bg {
		background: var(--dark-primary);
		border: 1.05854px solid var(--primary);
		position: absolute;
		width: 230px;
		height: 329.46px;
		top: 12px;
		left: 12px;
		z-index: 1;
		display: none;
	}

	/* Hover */
	.owned-assets .auction-card:hover .content .auction-btns .auction-btn-explore {
		padding-top: 10px;
		padding-bottom: 10px;
		font-size: 15px;
	}

	.owned-assets .auction-card:hover {
		margin-bottom: 0px;
	}

	.owned-assets .auction-card:hover .bg {
		display: block;
		z-index: -1;
	}
</style>
