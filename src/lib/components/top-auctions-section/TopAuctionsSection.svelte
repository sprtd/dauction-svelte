<script lang="ts">
	import type { Auction } from '$lib/interfaces';

	const auctions: Auction[] = [
		{
			id: Math.floor(1000 + Math.random() * 9000),
			profile_name: 'Strange Connections',
			profile_desc: 'Strange Connections #1414',
			profile_pic: '/images/profile-images/Strange-Connections.png',
			nft: '/images/hero-nft-2.png',
			crypto_price: 85.2,
			usd_price: 122029,
			liked: false
		},
		{
			id: Math.floor(1000 + Math.random() * 9000),
			profile_name: 'Bored Ape Yacht Club',
			profile_desc: 'BoredApeYachtClub #8867',
			profile_pic: '/images/profile-images/Bored-Ape-Yach-Club.png',
			nft: '/images/hero-nft-1.png',
			crypto_price: 95.2,
			usd_price: 125029,
			liked: false
		},
		{
			id: Math.floor(1000 + Math.random() * 9000),
			profile_name: 'ENS: Ethereum Name Service',
			profile_desc: 'okayduckdeployer.eth',
			profile_pic: '/images/profile-images/Ethereum Name Service.png',
			nft: '/images/nft-3.svg',
			crypto_price: 30,
			usd_price: 15029,
			liked: false
		},
		{
			id: Math.floor(1000 + Math.random() * 9000),
			profile_name: 'GardenLockdown',
			profile_desc: 'Paprika #4795',
			profile_pic: '/images/profile-images/GardenLockdown.png',
			nft: '/images/nft-4.svg',
			crypto_price: 183.9,
			liked: false,
			usd_price: 39657
		}
	];
</script>

<div class="top-auctions-section">
	<div class="top">
		<div class="wrapper">
			<div class="top-auctions-container">
				<h1>Top Auctions This Week</h1>
				<button class="btn-outline-primary top-auctions-btn">
					<span>See all auctions</span>
					<img src="/icons/arrow-forward-primary.svg" alt="" />
				</button>
			</div>
		</div>
	</div>
	<div class="bottom">
		<div class="wrapper">
			<div class="top-auctions">
				<!-- Auction -->
				{#each auctions as auction}
					<div class="auction">
						<div class="auction-card">
							<div class="content">
								<div class="auction-card-top">
									<div class="left">
										<span>{auction.profile_name}</span>
										<p>{auction.profile_desc}</p>
									</div>
									<img src={auction.profile_pic} alt="" />
								</div>
								<img src={auction.nft} class="auction-card-image" alt="" />
								<div class="auction-card-bottom">
									<div class="left">
										<span>Base Bid</span>
										<h4>{auction.crypto_price.toLocaleString()}<span>MATIC</span></h4>
										<span class="usd">${auction.usd_price.toLocaleString()}</span>
									</div>
									<div class="right">
										<span>Remaning Time</span>
										<h4>09h 24m 02s</h4>
										<p><span class="usd">24</span>bids placed</p>
									</div>
								</div>
								<div class="auction-btns">
									<button class="btn-primary auction-btn-place">
										<span>Place a Bid</span>
									</button>
									<button class="btn-outline-primary auction-btn-explore">
										<span>View</span>
									</button>
								</div>
							</div>
							<div class="bg " />
						</div>
					</div>
				{/each}
			</div>
		</div>
	</div>
</div>

<style>
	/* .top-auctions-section {
		background-image: url('/images/backgrounds/top-auctions-bg.png');
		background-repeat: no-repeat;
		background-position: center top;
		background-size: cover;
	} */

	.auction-card .bg {
		background: #1d220d;
		border: 1.05854px solid #d3f85a;
		position: absolute;
		width: 282px;
		height: 474.46px;
		top: 12px;
		left: 12px;
		z-index: 1;
		display: none;
	}

	/* Hover */
	.auction-card:hover .content .auction-btns .auction-btn-place,
	.auction-card:hover .content .auction-btns .auction-btn-explore {
		padding-top: 18px;
		padding-bottom: 18px;
	}

	.auction-card:hover .bg {
		display: block;
	}
</style>
