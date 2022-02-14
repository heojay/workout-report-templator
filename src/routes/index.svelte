<script lang="ts">
	import Button, { Label, Icon } from '@smui/button';
	import Paper, { Title, Subtitle, Content } from '@smui/paper';
	import Textfield from '@smui/textfield';
	import FormField from '@smui/form-field';
	import Slider from '@smui/slider';

	class MusclePain {
		id: number;
		area: string;
		scale: number;

		constructor(id: number) {
			this.id = id;
			this.area = '';
			this.scale = 5;
		}
	}

	const MIN_PAIN = 1;
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

	const shareReport = () => {
		if (navigator.share) {
			navigator.share({ text: getMessage() });
		} else {
			navigator.clipboard.writeText(getMessage()).then(() => alert('클립보드에 복사되었습니다.'));
		}
	};
</script>

<div class="paper-container">
	<Paper>
		<Title>공복 체중</Title>
		<Content>
			<Textfield
				type="number"
				bind:value={morningWeight}
				suffix="kg"
				style="width: 100%;"
				helperLine$style="width: 100%;"
			/>
		</Content>
	</Paper>
	<Paper>
		<Title>전일 운동</Title>
		<Content>
			<Textfield
				type="string"
				bind:value={yesterdayWorkout}
				style="width: 100%;"
				helperLine$style="width: 100%;"
			/>
		</Content>
	</Paper>
	<Paper>
		<Title>근육통 점수</Title>
		<Content>
			<div>
				<FormField align="end" style="display: flex;">
					<Button on:click={addMusclePain} variant="outlined">
						<Icon class="material-icons">add_box</Icon>
						<Label>부위 추가</Label></Button
					>
				</FormField>
			</div>
			{#each musclePains as { id, area, scale } (id)}
				<Paper>
					<Subtitle
						><Textfield
							type="string"
							bind:value={area}
							style="width: 100%;"
							helperLine$style="width: 100%;"
							label="부위"
						/></Subtitle
					>
					<div>
						<FormField align="end" style="display: flex;">
							<Slider
								style="flex-grow: 1;"
								bind:value={scale}
								min={MIN_PAIN}
								max={MAX_PAIN}
								discrete
							/>
							<span slot="label" style="padding-right: 12px; width: max-content; display: block;">
								{scale}/{MAX_PAIN}
							</span>
						</FormField>
					</div>
				</Paper>
			{/each}
		</Content>
	</Paper>
	<Paper>
		<Title>근육 상태 요약</Title>
		<Content>
			<Textfield
				textarea
				bind:value={muscleCondition}
				style="width: 100%;"
				helperLine$style="width: 100%;"
			/>
		</Content>
	</Paper>
</div>
<div class="container">
	<FormField align="end" style="display: flex;"
		><Button on:click={shareReport} variant="raised">
			<Icon class="material-icons">content_paste</Icon>
			<Label>공유하기</Label></Button
		>
	</FormField>
</div>
