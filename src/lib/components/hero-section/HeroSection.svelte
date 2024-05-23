<script>
	import { goto } from '$app/navigation';
</script>

<div class="hero-section">
	<div class="wrapper">
		<section class="hero-container">
			<div class="hero-text">
				<h1>
					Buy & Sell your Digital Assets in a
					<span>Sealed-Bidding Auction</span>
				</h1>
				<div class="hero-btns">
					<button class="btn-primary hero-btn-explore" on:click={() => goto('/explore')}>
						<span>Explore</span>
						<img src="/icons/arrow-forward-black.svg" alt="" />
					</button>
					<button
						class="btn-outline-primary hero-btn-create-auction"
						on:click={() => goto('/user/create-auction')}
					>
						<span>Create Auction</span>
					</button>
				</div>
			</div>
			<div class="hero-animation">
				<!-- Auction Card -->
				<div class="auction-card-wrapper front-image">
					<div class="auction-card">
						<div class="auction-card-top">
							<div class="left">
								<span>BoredApeYachtClub</span>
								<p>BoredApeYachtClub #5935</p>
							</div>
							<img src="/images/profile-images/Bored-Ape-Yach-Club.png" alt="" />
						</div>
						<img src="/images/hero-nft-1.png" class="auction-card-image" alt="" />
						<div class="auction-card-bottom">
							<div class="left">
								<span>Base Bid</span>
								<h4>146,880<span>MATIC</span></h4>
								<span class="usd">$122,029</span>
							</div>
							<div class="right">
								<span>Remaning Time</span>
								<h4>09h 24m 02s</h4>
								<p><span class="usd">24</span>bids placed</p>
							</div>
						</div>
					</div>
					<div class="bg" />
				</div>

				<!-- Auction Card -->
				<div class="auction-card-wrapper back-image">
					<div class="auction-card">
						<div class="auction-card-top">
							<div class="left">
								<span>Strange Connections</span>
								<p>Strange Connections #1414</p>
							</div>
							<img src="/images/profile-images/Strange-Connections.png" alt="" />
						</div>
						<img src="/images/hero-nft-2.png" class="auction-card-image" alt="" />
						<div class="auction-card-bottom">
							<div class="left">
								<span>Base Bid</span>
								<h4>85.20<span>MATIC</span></h4>
								<span>$122,029</span>
							</div>
							<div class="right">
								<span>Remaning Time</span>
								<h4>09h 24m 02s</h4>
								<p><span class="usd">24</span>bids placed</p>
							</div>
						</div>
					</div>
					<div class="bg" />
				</div>
				<img class="star-tl" src="icons/hero-star-tl.svg" alt="" />
				<img class="star-tr" src="icons/hero-star-tr.svg" alt="" />
				<img class="star-b" src="icons/hero-star-b.svg" alt="" />
			</div>
		</section>
	</div>
</div>

<style>
	/* .hero-section {
		background-image: url('images/backgrounds/hero-bg.png');
		background-repeat: no-repeat;
		background-position: center top;
		background-size: cover;
	} 
  
    .hero-section {
	padding-bottom: 70px !important;
  margin-bottom: 0;
}
  */
</style>
