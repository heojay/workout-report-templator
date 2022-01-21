<script>
	class MusclePain {
		constructor(id) {
			this.id = id;
			this.area = '';
			this.scale = 5;
		}
	}

	const MIN_PAIN = 0;
	const MAX_PAIN = 10;

	const today = new Date();
	const date = today.getMonth() + 1 + '/' + today.getDate();

	let morningWeight = '';
	let yesterdayWorkout = '';
	let muscleCondition = '';
	$: musclePains = [new MusclePain(0)];

	const addMusclePain = () => {
		const id = musclePains.length;
		musclePains = [...musclePains, new MusclePain(id)];
	};

	function getMessage() {
		const musclePainMessage = musclePains
			.map((e) => {
				return `  - ${e.area}: ${e.scale}/${MAX_PAIN}`;
			})
			.join('\r\n');
		return `${date} 보고
- 공복체중: ${morningWeight}kg
- 전일운동: ${yesterdayWorkout}
- 근육통점수:
${musclePainMessage}
- 근육상태요약:
${muscleCondition}`;
	}

	const copyToClipboard = () => {
		navigator.clipboard.writeText(getMessage()).then(() => alert('Copied'));
	};
</script>

<p>Daily Workout Template</p>

<ul>
	<li>오늘 날짜: {date}</li>
	<li>공복체중: <input type="number" bind:value={morningWeight} />kg</li>
	<li>전일운동: <input type="string" bind:value={yesterdayWorkout} /></li>
	<li>
		근육통점수 <button on:click={addMusclePain}>+</button>
		<ul>
			{#each musclePains as { id, area, scale } (id)}
				<li>
					<input type="string" bind:value={area} placeholder="부위" /><input
						type="range"
						bind:value={scale}
						min={MIN_PAIN}
						max={MAX_PAIN}
					/>{scale}/{MAX_PAIN}
				</li>
			{/each}
		</ul>
	</li>
	<li>근육상태요약:<br /><textarea bind:value={muscleCondition} /></li>
</ul>

<button on:click={copyToClipboard}>Copy to Clipboard</button>

<style>
	input,
	textarea {
		font-size: 16px;
	}
</style>
