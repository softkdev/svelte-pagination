<script lang="ts">
	const pageData = $state({
		currentPage: 1,
		totalPage: 18
	});

	const visiblePages = () => {
		if (pageData.totalPage <= 6) {
			return Array.from({ length: pageData.totalPage }, (_, i) => ({
				value: i + 1,
				class: ''
			}));
		}

		const pages = [];
		const currentItem = pageData.currentPage;
		const remainItem = pageData.totalPage - currentItem;
		let smallNumber = 7;

		if (remainItem < smallNumber) {
			const previousNumber = smallNumber - remainItem;
			if (previousNumber > 4) {
				smallNumber = previousNumber;
			} else {
				smallNumber = 4;
			}
		} else if (currentItem > 1) {
			const previousNumber = smallNumber - (currentItem - 1);
			if (previousNumber > 4) {
				smallNumber = previousNumber;
			} else {
				smallNumber = 4;
			}
		}
		const lessNumber = smallNumber - 1;
		const minNumber = lessNumber - 1;

		for (let i = 1; i <= pageData.totalPage; i++) {
			switch (true) {
				case i + smallNumber < currentItem:
					break;
				case i - smallNumber > currentItem:
					break;
				case i + lessNumber < currentItem:
					pages.push({
						value: null,
						class: 'smaller'
					});
					break;
				case i - lessNumber > currentItem:
					pages.push({
						value: null,
						class: 'smaller'
					});
					break;
				case i + minNumber < currentItem:
					pages.push({
						value: null,
						class: 'ellipsis'
					});
					break;
				case i - minNumber > currentItem:
					pages.push({
						value: null,
						class: 'ellipsis'
					});
					break;
				default:
					pages.push({
						value: i,
						class: ''
					});
			}
		}
		return pages;
	};
	const handleNextPage = () => {
		pageData.currentPage++;
	};

	const handlePreviousPage = () => {
		pageData.currentPage--;
	};

	const pageList = $derived(visiblePages());
</script>

<section class="flex w-full flex-col justify-between gap-5">
	<div>
		<p>Current Page: {pageData.currentPage}</p>
		<p>Total Page: {pageData.totalPage}</p>
	</div>
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
	<div class="flex items-center justify-center gap-2">
		{#each pageList as page, index (index + '' + page.value)}
			<button
				aria-label="pagination-item"
				onclick={() => {
					if (page.value) pageData.currentPage = page.value;
				}}
				class="dotItem {page.class} {pageData.currentPage === page.value ? 'activeDot' : ''}"
			></button>
		{/each}
	</div>
</section>

<style>
	.ellipsis,
	.smaller,
	.dotItem {
		@apply flex cursor-pointer rounded-full bg-gray-200 transition-all duration-300;
	}
	.dotItem {
		@apply h-4 w-4;
		&.activeDot {
			@apply w-8 bg-blue-500;
		}
	}
	.ellipsis,
	.smaller {
		@apply cursor-auto;
	}
	.ellipsis {
		@apply h-3 w-3;
	}
	.smaller {
		@apply h-2 w-2;
	}
	.previousBtn {
		@apply rounded-lg bg-gray-600 px-6 py-1 text-white transition-all disabled:cursor-not-allowed disabled:bg-gray-200 disabled:text-gray-400;
	}
</style>
