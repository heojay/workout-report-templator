<script lang="ts">
	import TopAppBar, {
		Row,
		Section,
		Title,
		AutoAdjust,
		TopAppBarComponentDev
	} from '@smui/top-app-bar';
	import IconButton from '@smui/icon-button';
	import { Icon } from '@smui/common';
	import { Svg } from '@smui/common/elements';
	import { mdiGithub } from '@mdi/js';

	let topAppBar: TopAppBarComponentDev;
	let lightTheme =
		typeof window === 'undefined' || window.matchMedia('(prefers-color-scheme: light)').matches;
	function switchTheme() {
		lightTheme = !lightTheme;
		let themeLink = document.head.querySelector<HTMLLinkElement>('#theme');
		if (!themeLink) {
			themeLink = document.createElement('link');
			themeLink.rel = 'stylesheet';
			themeLink.id = 'theme';
		}
		themeLink.href = `/smui${lightTheme ? '' : '-dark'}.css`;
		document.head
			.querySelector<HTMLLinkElement>('link[href="/smui-dark.css"]')
			?.insertAdjacentElement('afterend', themeLink);
	}
</script>

<TopAppBar bind:this={topAppBar} variant="static">
	<Row>
		<Section>
			<Title>일간 운동 보고서</Title>
		</Section>
		<Section align="end" toolbar>
			<IconButton on:click={switchTheme}>
				<Icon class="material-icons">{lightTheme ? 'light_mode' : 'dark_mode'}</Icon>
			</IconButton>
			<IconButton aria-label="GitHub" href="https://github.com/heojay/workout-report-templator">
				<Icon component={Svg} viewBox="0 0 24 24">
					<path d={mdiGithub} />
				</Icon>
			</IconButton>
		</Section>
	</Row>
</TopAppBar>

<AutoAdjust {topAppBar}>
	<div><slot /></div>
</AutoAdjust>
