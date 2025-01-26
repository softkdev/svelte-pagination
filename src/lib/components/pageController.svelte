<script>
	import { fly } from 'svelte/transition';
	import { Tween } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	let { previousActive, currentPage = $bindable(), ...pageData } = $props();

	const progress = new Tween(currentPage, {
		duration: 500,
		easing: cubicOut
	});

	$effect.pre(() => {
		progress.target = currentPage;
	});
	const currentItem = $derived(Math.round(progress.current));
	const visiblePages = () => {
		if (pageData.totalPage <= 6) {
			return Array.from({ length: pageData.totalPage }, (_, i) => ({
				value: i + 1,
				class: ''
			}));
		}

		const pages = [];
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
				case i - smallNumber > currentItem:
					break;
				case i + lessNumber < currentItem:
				case i - lessNumber > currentItem:
					pages.push({
						value: i,
						class: 'smaller'
					});
					break;
				case i + minNumber < currentItem:
				case i - minNumber > currentItem:
					pages.push({
						value: i,
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

	const pageList = $derived(visiblePages());
</script>

<div class="pagination-container no-scrollbar flex snap-x snap-mandatory items-center gap-2">
	{#each pageList as page (page.value)}
		<button
			in:fly={{
				duration: 100,
				x: currentItem > previousActive ? -100 : 100
			}}
			out:fly={{
				duration: 100,
				x: currentItem > previousActive ? -10 : 10
			}}
			data-page={page.value}
			aria-label="pagination-item"
			onclick={() => {
				currentPage = page.value;
			}}
			class="dotItem {page.class} {currentItem === page.value ? 'activeDot' : ''}"
		></button>
	{/each}
</div>

<style>
	.pagination-container {
		@apply w-[200px] overflow-x-hidden;
	}

	.ellipsis,
	.smaller,
	.dotItem {
		@apply cursor-pointer rounded-full bg-gray-200 transition-all duration-300;
	}
	.dotItem {
		@apply h-4 min-w-4;
		&.activeDot {
			@apply min-w-8 snap-center bg-blue-500;
		}
	}
	.ellipsis {
		@apply h-3 min-w-3;
	}
	.smaller {
		@apply h-2 min-w-2;
	}
</style>
