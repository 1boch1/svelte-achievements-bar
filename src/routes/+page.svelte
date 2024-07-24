<script>
	import { AchievementsBar } from '$lib/index.js';

	let achievements = [
		{ icon: 'skull', text: 'Start' },
		{ icon: 'album', text: 'Intermediate' },
		{ icon: 'usb', text: 'Advanced' },
		{ icon: 'book', text: 'Expert' }
	];

	let currentProgress = 2.4;
</script>

<AchievementsBar
	{achievements}
	barHeight="1rem"
	iconSize="1.4rem"
	textSize="15px"
	progress={currentProgress}
	bgColor="#8C93A8"
	mainColor="#4CAF50"
/>
