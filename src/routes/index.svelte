<script lang="ts">
	import Button, { Label, Icon } from '@smui/button';
	import Paper, { Title, Subtitle, Content } from '@smui/paper';
	import Textfield from '@smui/textfield';
	import HelperText from '@smui/textfield/helper-text';
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

	let morningWeight: number = 0;
	let yesterdayWorkout: string = '';
	let muscleCondition: string = '';
	$: musclePains = [new MusclePain(0)];

	function addMusclePain() {
		const id = musclePains.length ? musclePains[musclePains.length - 1].id + 1 : 0;
		musclePains = [...musclePains, new MusclePain(id)];
	}

	function removeMusclePain(id: number) {
		musclePains = musclePains.filter((musclePain) => musclePain.id != id);
	}

	function validateInput(
		morningWeight: number,
		muscleCondition: string,
		musclePains: Array<MusclePain>
	): boolean {
		if (
			isNaN(morningWeight) ||
			morningWeight <= 0 ||
			muscleCondition === '' ||
			musclePains.some((musclePain) => musclePain.area === '')
		) {
			return false;
		}
		return true;
	}

	function getMessage() {
		const musclePainMessage = musclePains.length
			? '\n' +
			  musclePains
					.map((e) => {
						return `  - ${e.area}: ${e.scale}/${MAX_PAIN}`;
					})
					.join('\r\n')
			: '근육통 없음';
		return `${date} 보고
- 공복체중: ${morningWeight}kg
- 전일운동: ${yesterdayWorkout !== '' ? yesterdayWorkout : '없음'}
- 근육통점수: ${musclePainMessage}
- 근육상태요약:
${muscleCondition}`;
	}

	function shareReport() {
		if (navigator.share) {
			navigator.share({ text: getMessage() });
		} else {
			navigator.clipboard.writeText(getMessage()).then(() => alert('클립보드에 복사되었습니다.'));
		}
	}
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
			>
				<HelperText persistent slot="helper">화장실을 다녀온 직후의 몸무게</HelperText>
			</Textfield>
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
			>
				<HelperText persistent slot="helper">선택, 공백인 경우 "없음"</HelperText>
			</Textfield>
		</Content>
	</Paper>
	<Paper>
		<Title>근육통 점수</Title>
		<Content>
			<div>
				<FormField align="end" style="display: flex;">
					<Button on:click={addMusclePain} variant="outlined">
						<Icon class="material-icons">add_circle</Icon>
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
					<div align="end">
						<Button on:click={() => removeMusclePain(id)} align="end" variant="outlined">
							<Icon class="material-icons">remove_circle</Icon>
							<Label>부위 삭제</Label></Button
						>
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
			>
				<HelperText persistent slot="helper">근육 상태와 컨디션을 짧게 요약</HelperText>
			</Textfield>
		</Content>
	</Paper>
</div>
<div class="container">
	<FormField align="end" style="display: flex;"
		><Button
			disabled={!validateInput(morningWeight, muscleCondition, musclePains)}
			on:click={shareReport}
			variant="raised"
		>
			<Icon class="material-icons">content_paste</Icon>
			<Label>공유하기</Label></Button
		>
	</FormField>
</div>
