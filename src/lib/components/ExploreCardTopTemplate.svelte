<script lang="ts">
	import { AVAILABLE_AUCTIONS } from '$lib/stores/main';
	import { sortArrayofObjects } from '$lib/utils/otherUtils';

	export let profile_name: string;
	export let profile_desc: string;
	export let profile_pic: string;
	export let nft: string;
	export let liked: boolean;
	export let tokenId: number;

	const handleLikeAuction = (tokenId: number) => {
		let newAuction = $AVAILABLE_AUCTIONS.find((x) => x.tokenId === tokenId);
		if (newAuction) {
			newAuction.liked = !newAuction.liked;
			AVAILABLE_AUCTIONS.set(
				sortArrayofObjects(
					[...$AVAILABLE_AUCTIONS.filter((x) => x.tokenId !== tokenId), newAuction],
					'tokenId'
				)
			);
		}
	};
</script>

<div class="card-top-template-top">
	<div class="auction-card-top">
		<div class="left">
			<span>{profile_name}</span>
			<p>{profile_desc}</p>
		</div>
		<img src={profile_pic} alt="" />
	</div>
	<img src={nft} class="auction-card-image" alt="" />
	{#if liked}
		<img
			src="/icons/full-heart.svg"
			alt=""
			class="liked"
			on:click={() => handleLikeAuction(tokenId)}
		/>
	{:else}
		<img
			src="/icons/empty-heart.svg"
			alt=""
			class="liked"
			on:click={() => handleLikeAuction(tokenId)}
		/>
	{/if}
</div>

<style>
	* {
		font-family: 'Darker Grotesque', sans-serif;
	}

	.card-top-template-top .auction-card-top .left span {
		font-size: 14px;
	}
	.card-top-template-top .auction-card-image {
		width: 250px;
		height: 241px;
		margin-top: 13.35px;
	}
	.card-top-template-top .liked {
		cursor: pointer;
		position: absolute;
		width: 24.57px;
		height: 24.57px;
		right: 23px;
		top: 67px;
	}
	.card-top-template-top .auction-card-top .left p {
		font-size: 16px;
	}
	.card-top-template-top .auction-card-top img {
		width: 36px;
		height: 36px;
	}
</style>
