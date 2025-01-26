<script lang="ts">
	import PageController from '$lib/components/pageController.svelte';

	const pageData = $state({
		currentPage: 1,
		totalPage: 18,
		previousActive: 1
	});

	const handleNextPage = () => {
		pageData.previousActive = pageData.currentPage;
		pageData.currentPage++;
	};

	const handlePreviousPage = () => {
		pageData.previousActive = pageData.currentPage;
		pageData.currentPage--;
	};

	const handleUpdateValue = (value: number) => {
		pageData.previousActive = pageData.currentPage;
		if (value > 0 && value <= pageData.totalPage) {
			pageData.currentPage = value;
		}
	};
</script>

<section class="flex w-full flex-col justify-between gap-5">
	<div class="flex flex-col gap-5">
		<div class="flex items-center gap-5">
			<label for="currentPage"> Current Page: </label>
			<input
				id="currentPage"
				class="w-52 rounded-md border border-gray-300 px-4 py-1.5 outline-none"
				placeholder="Current Page"
				min={1}
				max={pageData.totalPage}
				oninput={(e) => handleUpdateValue(Number(e.currentTarget.value))}
				value={pageData.currentPage}
				type="number"
			/>
		</div>
		<div class="flex items-center gap-5">
			<label for="totalPage"> Total Page:</label>
			<input
				id="totalPage"
				min={1}
				class="w-52 rounded-md border border-gray-300 px-4 py-1.5 outline-none"
				placeholder="Current Page"
				bind:value={pageData.totalPage}
				type="number"
			/>
		</div>
	</div>
	<div></div>
	<div class="flex gap-4">
		<button class="previousBtn" disabled={pageData.currentPage === 1} onclick={handlePreviousPage}>
			Previous
		</button>
		<button
			class="previousBtn"
			disabled={pageData.currentPage === pageData.totalPage}
			onclick={handleNextPage}
		>
			Next
		</button>
	</div>
	<PageController {...pageData} bind:currentPage={pageData.currentPage} />
</section>

<style>
	.previousBtn {
		@apply rounded-lg bg-gray-600 px-6 py-1 text-white transition-all disabled:cursor-not-allowed disabled:bg-gray-200 disabled:text-gray-400;
	}
</style>
