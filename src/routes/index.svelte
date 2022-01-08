<script>
	const today = new Date();
	const date = today.getMonth() + 1 + '/' + today.getDate();

	let morningWeight = '';
	let yesterdayWorkout = '스텝밀';
	let muscleCondition = '';
	let musclePain = '';

	function getMessage() {
		return `${date} 보고
- 공복체중: ${morningWeight}kg
- 전일운동: ${yesterdayWorkout}
- 근육상태: ${muscleCondition}
- 근육통점수: ${musclePain}`;
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
	<li>근육상태: <input type="textarea" bind:value={muscleCondition} /></li>
	<li>근육통점수: <input type="string" bind:value={musclePain} /></li>
</ul>

<button on:click={copyToClipboard}>Copy to Clipboard</button>
