<script lang="ts">
	import { SlideToggle } from '@skeletonlabs/skeleton';
	let count = 0;
	let adding = 1;
	let canAffordUpgrade = false;
	let upgradeCost = 10;
	let multiplier = 1;
	let multiplierCost = 10000;
	let canAffordMultiplier = false;

	let autoClickOn = false;
	let autoClickInterval: ReturnType<typeof setInterval> | undefined;

	let autoUpgradeOn = false;
	let autoUpgradeInterval: ReturnType<typeof setInterval> | undefined;

	let autoMultiplyOn = false;
	let autoMultiplyInterval: ReturnType<typeof setInterval> | undefined;

	function increment() {
		count += adding * multiplier;
	}

	function Reset() {
		const userConfrimed = confirm('Are you sure you want to reset?');
		if (userConfrimed) {
			count = 0;
			adding = 1;
			upgradeCost = 10;
			multiplier = 1;
			multiplierCost = 10000;

			autoClickOn = false;
			autoMultiplyOn = false;
			autoUpgradeOn = false;
		}
	}

	function upgrade() {
		if (count >= upgradeCost) {
			count -= upgradeCost;
			adding += 1;
			upgradeCost = Math.trunc(upgradeCost + upgradeCost * 0.6);
		}
	}

	function multiply() {
		if (count >= multiplierCost) {
			count -= multiplierCost;
			multiplier += multiplier * 0.5;
			multiplierCost = Math.trunc(multiplierCost + multiplierCost * 0.6);
		}
	}

	$: {
		if (autoClickOn) {
			if (!autoClickInterval) {
				autoClickInterval = setInterval(increment, 10);
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
	$: {
		if (autoMultiplyOn) {
			if (!autoMultiplyInterval) {
				autoMultiplyInterval = setInterval(multiply, 100);
			}
		} else {
			if (autoMultiplyInterval) {
				clearInterval(autoMultiplyInterval);
				autoMultiplyInterval = undefined;
			}
		}
	}
	$: canAffordUpgrade = count >= upgradeCost;
	$: canAffordMultiplier = count >= multiplierCost;
</script>

<div class="flex flex-col items-center text-center">
	<h1>{Math.trunc(count)}$</h1>
	<div class="grid grid-cols-2 gap-3 p-4">
		<button type="button" on:click={increment} class="variant-ghost-primary p-2 rounded-lg"
			>Increment</button
		>
		<button
			type="button"
			on:click={upgrade}
			class=" p-2 rounded-lg variant-ghost-primary"
			class:variant-ghost-error={!canAffordUpgrade}>upgrade</button
		>
		<button
			type="button"
			on:click={multiply}
			class=" p-2 rounded-lg variant-ghost-primary"
			class:variant-ghost-error={!canAffordMultiplier}>multiply</button
		>
	</div>
</div>

<div class="flex flex-col gap-3 absolute top-0 right-0 p-4 border-solid variant-ghost-error">
	<SlideToggle name="slider-label" bind:checked={autoClickOn} active="bg-primary-500" size="sm"
		>Autoclicker</SlideToggle
	>
	<SlideToggle name="slider-label" bind:checked={autoUpgradeOn} active="bg-primary-500" size="sm"
		>AutoUpgrade</SlideToggle
	>
	<SlideToggle name="slider-label" bind:checked={autoMultiplyOn} active="bg-primary-500" size="sm"
		>AutoMultiply</SlideToggle
	>
	<button type="button" on:click={Reset} class="variant-filled-error p-2 rounded-lg">Reset</button>
</div>
