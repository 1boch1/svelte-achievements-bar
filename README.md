# svelte-achievements-bar

`svelte-achievements-bar` is a customizable Svelte component that displays a progress bar with icons and text, indicating the progress of various achievements. The component utilizes the `lucide-svelte` library for icons.

## Result

![IMG_4998](https://github.com/user-attachments/assets/2660e445-c2b8-4fd4-bc99-92c0ca63d6e9)

## Technologies Used

- **Svelte**: The front-end framework for building the component.
- **lucide-svelte**: A library for using Lucide icons in Svelte components.
- **Vite**: For module bundling and handling dynamic imports.

## Installation

To use the `svelte-achievements-bar` component in your project, you need to install it along with its dependencies:

```bash
npm install svelte-achievements-bar lucide-svelte
```

## Usage

Here is a basic example of how to use the `svelte-achievements-bar` component in your Svelte application.

### App.svelte

```svelte
<script>
	import AchievementsBar from 'svelte-achievements-bar';

	let achievements = [
		{ icon: 'skull', text: 'Start' },
		{ icon: 'album', text: 'Intermediate' },
		{ icon: 'usb', text: 'Advanced' }
	];

	let progress = 1.3; // This means the user has completed the first achievement
</script>

<AchievementsBar {achievements} {progress} />
```

### Props

- `achievements`: An array of achievement objects. Each object should contain an `icon` and `text` property.
  - `icon`: The name of the icon to display (from Lucide).
  - `text`: The text to display under the icon.
- `barHeight`: The height of the progress bar. Default is `'20px'`.
- `iconSize`: The size of the icons. Default is `'40px'`.
- `textSize`: The size of the text under the icons. Default is `'16px'`.
- `iconStroke`: The stroke width of the icons. Default is `1.5`.
- `iconColor`: The color of the icons. Default is `'#050505'`.
- `iconLitColor`: The color of the lit icons (line color).
- `bgColor`: The background color of the progress bar. Default is `'#8C93A8'`.
- `mainColor`: The color of the progress bar and the lit icons. Default is `'#4CAF50'`.
- `progress`: The current progress as a number from 0 to `(achievements.length - 1)`.

## Examples

### Example with Custom Styling

```svelte
<script>
	import AchievementsBar from 'svelte-achievements-bar';

	let achievements = [
		{ icon: 'skull', text: 'Start' },
		{ icon: 'album', text: 'Intermediate' },
		{ icon: 'usb', text: 'Advanced' }
	];
	let progress = 2;
</script>

<AchievementsBar
	{achievements}
	{progress}
	barHeight="30px"
	iconSize="50px"
	textSize="18px"
	iconStroke="2"
	iconColor="#ffffff"
	bgColor="#333"
	mainColor="#ff6347"
/>
```

## Explanation

- **Dynamic Icon Loading**: The icons are dynamically imported using Vite's module bundling. This allows for efficient loading and reduces the initial load time.
- **Progress Bar**: The progress bar visually represents the user's progress through the achievements.
- **Lit Icons**: Icons change their background color based on the user's progress to indicate completed achievements.
- **Customizable Styles**: You can customize the height of the bar, the size and color of the icons, the size of the text, and the colors used for the background and progress bar.

## Styling

The component uses CSS variables for styling, which allows for easy customization:

- `--bar-height`: Height of the progress bar.
- `--bg-color`: Background color of the progress bar and icons.
- `--main-color`: Color of the progress bar and lit icons.
- `--icon-size`: Size of the icons.
- `--text-size`: Size of the text.

```css
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
    padding: calc(var(--icon-size) / 2.5);
    border-radius: 50%;
    background-color: var(--bg-color);
    display: flex;
    justify-content: center;
    align-items: center;
    transition: background-color 0.3s;
  }

  .icon.lit {
    background-color: var(--main-color);
  }

  .text {
    font-size: var(--text-size);
    position: absolute;
    margin-top: calc(var(--icon-size) * 0.8);
  }
</style>
```

## Contributions

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

## Support

If you encounter any issues or have any questions, feel free to open an issue on GitHub.
