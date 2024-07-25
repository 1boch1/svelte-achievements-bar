<script>
	import { onMount } from 'svelte';
	import { writable } from 'svelte/store';

	/**
	 * Array of achievement objects. Each object should have an `icon` property
	 * specifying the name of the lucid-svelte desired icon (without extension) and a `text` property
	 * with the achievement description.
	 * @type {Array<{ icon: string, text: string }>}
	 */
	export let achievements = [];

	/**
	 * Height of the progress bar.
	 * @type {string}
	 * @default '20px'
	 */
	export let barHeight = '20px';

	/**
	 * Size of the achievement icons.
	 * @type {string}
	 * @default '40px'
	 */
	export let iconSize = '40px';

	/**
	 * Font size of the achievement text.
	 * @type {string}
	 * @default '16px'
	 */
	export let textSize = '16px';

	/**
	 * Stroke width of the icons.
	 * @type {number}
	 * @default 1.5
	 */
	export let iconStroke = '1.5';

	/**
	 * Default color of the icons.
	 * @type {string}
	 * @default '#050505'
	 */
	export let iconColor = '#050505';

	/**
	 * Color of the lit icons (line color, not background).
	 * @type {string}
	 * @default '#00AF50'
	 */
	export let iconLitColor = '#00AF50';

	/**
	 * Background color of the progress bar.
	 * @type {string}
	 * @default '#8C93A8'
	 */
	export let bgColor = '#8C93A8';

	/**
	 * Main color of the progress and icon background.
	 * @type {string}
	 * @default '#4CAF50'
	 */
	export let mainColor = '#4CAF50';

	/**
	 * Progress value, a number from 0 to (achievements.length - 1).
	 * @type {number}
	 * @default 0
	 */
	export let progress = 0;

	// Store for imported icons
	let importedIcons = writable([]);

	/**
	 * Load the icons dynamically based on the achievements array.
	 * If an icon fails to load, null is pushed to the icons array.
	 * @async
	 * @function loadIcons
	 */
	async function loadIcons() {
		const icons = [];

		for (const { icon } of achievements) {
			try {
				const Icon = (
					await import(
						/* @vite-ignore */
						`/node_modules/lucide-svelte/dist/icons/${icon}.svelte`
					)
				).default;

				icons.push(Icon);
			} catch (error) {
				console.error(`Failed to load icon: ${icon}`, error);
				icons.push(null); // Push null if import fails
			}
		}

		importedIcons.set(icons);
	}

	// Load icons when component mounts
	onMount(() => {
		loadIcons();
	});
</script>

<div class="progress-bar-container">
	<div class="progress-bar-wrapper">
		<div class="progress-bar" style="--bar-height: {barHeight}; --bg-color: {bgColor};">
			<div
				class="progress"
				style="width: {(progress / (achievements.length - 1)) * 100}%; --main-color: {mainColor};"
			></div>
		</div>

		{#await $importedIcons}
			<p>Loading...</p>
		{:then icons}
			<div class="achievements">
				{#each achievements as achievement, index}
					<div class="achievement" style="--icon-size: {iconSize}; --text-size: {textSize};">
						{#if icons[index]}
							<div
								class="icon {progress >= index ? 'lit' : ''}"
								style="--main-color: {mainColor}; --bg-color: {bgColor};"
							>
								<svelte:component
									this={icons[index]}
									size={iconSize}
									color={progress >= index ? iconLitColor : iconColor}
									strokeWidth={iconStroke}
								/>
							</div>
						{/if}
						<div class="text" style="top: calc({iconSize} + 1vw)">
							{achievement.text}
						</div>
					</div>
				{/each}
			</div>
		{/await}
	</div>
</div>

<style>
	.progress-bar-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 100%;
	}

	.progress-bar-wrapper {
		width: 100%;
		position: relative;
		display: flex;
		align-items: center;
	}

	.progress-bar {
		background-color: var(--bg-color);
		border-radius: 2vw;
		height: var(--bar-height);
		width: 100%;
		position: relative;
	}

	.progress {
		background-color: var(--main-color);
		height: 100%;
		width: 0;
		transition: width 0.3s;
		border-radius: 2vw;
	}

	.achievements {
		display: flex;
		justify-content: space-between;
		width: 100%;
		position: absolute;
	}

	.achievement {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
		position: relative;
	}

	.icon {
		padding: calc(var(--icon-size) / 2.4);
		border-radius: 50%;
		background-color: var(--bg-color); /* Background color for the icon circle */
		display: flex;
		justify-content: center;
		align-items: center;
		transition: background-color 0.3s;
	}

	.icon.lit {
		background-color: var(--main-color); /* Lit color */
	}

	.text {
		font-size: var(--text-size);
		position: absolute;
		margin-top: calc(var(--icon-size) * 0.8); /* Adjusted position */
	}
</style>
