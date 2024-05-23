<script lang="ts">
	import {
		connected,
		chainId,
		selectedAccount,
		defaultEvmStores as evm,
		web3,
		//@ts-ignore
		contracts
	} from 'svelte-web3';
	import { onMount } from 'svelte';

	import { openModal, closeModal } from 'svelte-modals';
	import LoadingModal from '$lib/components/modals/LoadingModal.svelte';
	import { RANDOM_PROFILE } from '$lib/utils/constants';
	import ExploreCardTopTemplate from '$lib/components/ExploreCardTopTemplate.svelte';
	import { goto } from '$app/navigation';
	import CountdownTimer from '$lib/components/reusables/CountdownTimer.svelte';
	import { datetoUnix, unixToDate } from '$lib/utils/timeUtils';
	import { AVAILABLE_AUCTIONS, currentAuction, NEW_AUCTION_CHANGES } from '$lib/stores/main';
	import { formatPrice, fromWei } from '$lib/utils/conversionUtils';
	import { arrayIsNotEqual, sortArrayofObjects } from '$lib/utils/otherUtils';

	import { toast } from '@zerodevx/svelte-toast';
	import { errorToast, successToast } from '$lib/components/toast/toastTheme';

	onMount(() => {
		// console.log($AVAILABLE_AUCTIONS[0]?.bidders_);
		NEW_AUCTION_CHANGES.set(true);
	});

	$: if ($AVAILABLE_AUCTIONS.length > 0) {
		// errorToast('It works!');
	}

	$: if ($connected) {
		// getNFTImage($contracts);
	}

	const handlePlaceBid = async (auction: any) => {
		await currentAuction.set(auction);
		goto(`/user/bid-auction/${auction.tokenId}`);
	};

	const handleSettleAuction = async (auction: any) => {
		await currentAuction.set(auction);
		goto(`/user/settle-auction/${auction.tokenId}`);
	};

	const handleRevealBid = async (auction: any) => {
		await currentAuction.set(auction);
		goto(`/user/reveal-bid/${auction.tokenId}`);
	};
</script>

<div class="wrapper">
	<div class="explore-auctions-section">
		<div class="top">
			<h1>Explore Auctions</h1>
			<div class="toolbar">
				<div class="search-container">
					<input type="text" name="search" placeholder="Search by items or sellers" />
					<img src="/icons/search-icon.svg" alt="" />
				</div>
				<select name="catergory">
					<option value="1">Catergory 1</option>
				</select>
				<select name="Price">
					<option value="1">Price 1</option>
				</select>
			</div>
		</div>
		<!-- <div class="auctions">Auction</div> -->
		{#if $connected}
			<!-- <div style="color: red; font-size:30px" class="" /> -->
			<!-- {#await getNFTImage($contracts?.nftContract, '2')}
			<p>...waiting</p>
		{:then data}
			<img src={data} alt="Dog" width="500px" />
		{:catch error}
			<p>An error occurred!</p>
		{/await} -->

			<div class="explore">
				<div class="auctions-container">
					{#each sortArrayofObjects($AVAILABLE_AUCTIONS, 'tokenId') as auction}
						<div class="auction">
							<div class="auction-card">
								<div class="content">
									<ExploreCardTopTemplate
										profile_name={'auction.profile_name'}
										profile_desc={'auction.profile_desc'}
										profile_pic={RANDOM_PROFILE[Math.floor(Math.random() * RANDOM_PROFILE.length)]}
										nft={auction.image}
										liked={auction.liked}
										tokenId={auction.tokenId}
									/>
									<div class="auction-card-bottom">
										<div class="left">
											<span>Base Bid</span>
											<h4>${formatPrice(auction.minBidPrice)}</h4>
											<span class="usd" style="font-weight: 700;margin-top: 2px">
												{`Token ID - [${auction.tokenId}]`}
											</span>
										</div>
										<div class="right">
											{#if datetoUnix(new Date()) < auction.startTime}
												<span>Time before bid starts</span>
												<h4>
													<CountdownTimer endTime={unixToDate(auction.startTime)} />
												</h4>
											{:else if $selectedAccount && auction.bidders
													.join('')
													.toLowerCase()
													.includes($selectedAccount?.toLowerCase()) && datetoUnix(new Date()) > auction.endTime}
												<span>Time left to reveal</span>
												<h4>
													<CountdownTimer endTime={unixToDate(auction.revealDuration)} />
												</h4>
											{:else if $selectedAccount && auction.owner.toLowerCase() === $selectedAccount?.toLowerCase() && datetoUnix(new Date()) > auction.endTime}
												<span>Time left to Settle</span>
												<h4>
													<CountdownTimer endTime={unixToDate(auction.revealDuration)} />
												</h4>
											{:else}
												<span>Remaning Time</span>
												<h4>
													<CountdownTimer endTime={unixToDate(auction.endTime)} />
												</h4>
											{/if}
											<p>
												<span class="usd">{auction.bidders.length}</span>{`${
													auction.bidders.length > 1 ? 'bids' : 'bid'
												} placed`}
											</p>
										</div>
									</div>
									<!-- {#if datetoUnix(new Date()) < auction.endTime}  disabled={datetoUnix(new Date()) >= auction.endTime}-->
									<div class="auction-btns">
										{#if $selectedAccount && auction.owner.toLowerCase() === $selectedAccount.toLowerCase()}
											<button
												class="btn-primary auction-btn-place"
												on:click={() => handleSettleAuction(auction)}
												disabled={datetoUnix(new Date()) < auction.revealDuration}
											>
												<span>Settle Auction</span>
											</button>
											<button class="btn-outline-primary auction-btn-explore" style="width: 45%;">
												<span>View</span>
											</button>
										{:else if $selectedAccount && auction.bidders
												.join('')
												.toLowerCase()
												.includes($selectedAccount?.toLowerCase())}
											<button
												class="btn-primary auction-btn-place"
												on:click={() => handleRevealBid(auction)}
												disabled={!(
													datetoUnix(new Date()) > auction.endTime &&
													datetoUnix(new Date()) < auction.revealDuration
												)}
											>
												<span>Reveal Bid</span>
											</button>
											<button class="btn-outline-primary auction-btn-explore" style="width: 50%;">
												<span>View</span>
											</button>
											<!--{:else if !auction.bidders
											.join('')
											.toLowerCase()
											.includes($selectedAccount?.toLowerCase())}
											 <button
											class="btn-primary auction-btn-place"
											on:click={() => handlePlaceBid(auction)}
											disabled={datetoUnix(new Date()) >= auction.endTime}
										>
											<span>Place a Bid</span>
										</button> -->
										{:else}
											<button
												class="btn-primary auction-btn-place"
												on:click={() => handlePlaceBid(auction)}
												disabled={datetoUnix(new Date()) > auction.endTime ||
													datetoUnix(new Date()) < auction.startTime}
											>
												<span>Place a Bid</span>
											</button>
											<button class="btn-outline-primary auction-btn-explore" style="width: 50%;">
												<span>View</span>
											</button>
										{/if}
									</div>
									<!-- {/if} -->
								</div>
								<div class="bg " />
							</div>
						</div>
					{:else}
						<div class="title">
							<h1>No Auctions Yet</h1>
						</div>
					{/each}
				</div>
			</div>
		{/if}
	</div>
</div>

<style>
	.explore {
		width: 100%;
	}
	.explore .auctions-container {
		display: grid;
		grid-template-columns: repeat(auto-fill, 282px);
		grid-gap: 24px;
		z-index: 10;
		padding-bottom: 20px;
	}

	.explore .auctions-container .auction .auction-card {
		position: relative;
		z-index: 10;
	}

	.explore .auction-card {
		margin-bottom: 16px;
	}

	.explore .auction-card .content {
		padding: 16px;
	}

	.explore .auction-card .content .auction-card-bottom .left > span,
	.explore .auction-card .content .auction-card-bottom .right > span,
	.explore .auction-card .content .auction-card-bottom .right p {
		font-size: 14px;
	}

	.explore .auction-card .content .auction-card-bottom .right p span {
		font-size: 16px;
	}

	.explore .auction-card .content .auction-card-bottom .right p {
		font-weight: 700;
	}
	.explore .auction-card .content .auction-card-bottom .left h4,
	.explore .auction-card .content .auction-card-bottom .right h4 {
		font-size: 22px;
	}
	.explore .auction-card .content .auction-btns {
		align-items: normal;
	}
	.explore .auction-card .content .auction-btns .auction-btn-place,
	.explore .auction-card .content .auction-btns .auction-btn-explore {
		padding: 10px 20px;
		font-weight: 400;
		font-size: 14px;
	}

	.explore .auction-card .bg {
		background: #1d220d;
		border: 1.05854px solid #d3f85a;
		position: absolute;
		width: 282px;
		height: 459.46px;
		top: 12px;
		left: 12px;
		z-index: 1;
		display: none;
	}

	/* Hover */
	.explore .auction-card:hover .content .auction-btns .auction-btn-place,
	.explore .auction-card:hover .content .auction-btns .auction-btn-explore {
		padding-top: 10px;
		padding-bottom: 10px;
		font-size: 15px;
	}

	.explore .auction-card:hover {
		margin-bottom: 0px;
	}

	.explore .auction-card:hover .bg {
		display: block;
		z-index: -1;
	}

	.explore-auctions-section .title {
		text-align: center;
		width: 100%;
		color: var(--white-background);
		grid-column-end: span 4;
	}
</style>
