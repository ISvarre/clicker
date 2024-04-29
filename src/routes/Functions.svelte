<script lang="ts">
	import { SlideToggle } from '@skeletonlabs/skeleton';
	let count = 0;
	let adding = 1;
	let multiplier = 1;
	let canAfford = false;

	let autoClickOn = false;
	let autoClickInterval: ReturnType<typeof setInterval> | undefined;

	let autoUpgradeOn = false;
	let autoUpgradeInterval: ReturnType<typeof setInterval> | undefined;

	let upgradeCost = 10;

	function increment() {
		count += adding * multiplier;
	}

	function Reset() {
		const userConfrimed = confirm('Are you sure you want to reset?');
		if (userConfrimed) {
			count = 0;
			adding = 1;
			multiplier = 1;
			upgradeCost = 10;
		}
	}

	function upgrade() {
		if (count >= upgradeCost) {
			count -= upgradeCost;
			adding += 1;
			upgradeCost = Math.trunc(upgradeCost + upgradeCost * 0.6);
		}
	}

	$: {
		if (autoClickOn) {
			if (!autoClickInterval) {
				autoClickInterval = setInterval(increment, 100);
			}
		} else {
			if (autoClickInterval) {
				clearInterval(autoClickInterval);
				autoClickInterval = undefined;
			}
		}
	}
	$: {
		if (autoUpgradeOn) {
			if (!autoUpgradeInterval) {
				autoUpgradeInterval = setInterval(upgrade, 100);
			}
		} else {
			if (autoUpgradeInterval) {
				clearInterval(autoUpgradeInterval);
				autoUpgradeInterval = undefined;
			}
		}
	}
	$: canAfford = count >= upgradeCost;
</script>

<div class="flex flex-col items-center">
	<h1>{Math.trunc(count)}$</h1>
	<div class="grid grid-cols-2 gap-3 p-4">
		<button type="button" on:click={increment} class="variant-ghost-primary p-2 rounded-lg"
			>Increment</button
		>
		<button
			type="button"
			on:click={upgrade}
			class=" p-2 rounded-lg variant-ghost-primary"
			class:variant-ghost-error={!canAfford}>upgrade</button
		>
		<p>{adding}</p>
		<p>{upgradeCost}</p>
	</div>
	<div class="flex flex-col gap-3 absolute top-0 right-0 p-4 border-solid variant-ghost-error">
		<SlideToggle name="slider-label" bind:checked={autoClickOn} active="bg-primary-500" size="sm"
			>Autoclicker</SlideToggle
		>
		<SlideToggle name="slider-label" bind:checked={autoUpgradeOn} active="bg-primary-500" size="sm"
			>Autoclicker</SlideToggle
		>
		<button type="button" on:click={Reset} class="variant-filled-error p-2 rounded-lg">Reset</button
		>
	</div>
</div>
