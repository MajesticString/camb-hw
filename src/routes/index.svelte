<script lang="ts">
	import { VERSION } from 'svelte/compiler';

	interface CurrentAssignments {
		[key: string]: ClassAssignments;
	}
	interface ClassAssignments {
		mostUpvotedEntryIndex: 0 | number;
		entries: Entry[];
	}
	interface Entry {
		upvotes: number;
		downvotes: number;
		assignment: string;
	}
	const currentAssignments: CurrentAssignments = {
		sci: {
			mostUpvotedEntryIndex: 0,
			entries: [
				{
					upvotes: 50,
					downvotes: 10,
					assignment: '19.1 summary questions'
				},
				{
					upvotes: 30,
					downvotes: 100,
					assignment: 'Nothing'
				}
			]
		},
		alg: {
			mostUpvotedEntryIndex: 0,
			entries: [
				{
					upvotes: 50,
					downvotes: 10,
					assignment: '19.1 summary questions'
				},
				{
					upvotes: 30,
					downvotes: 100,
					assignment: 'Nothing'
				}
			]
		},
		geo: {
			mostUpvotedEntryIndex: 0,
			entries: [
				{
					upvotes: 50,
					downvotes: 10,
					assignment: '19.1 summary questions'
				},
				{
					upvotes: 30,
					downvotes: 100,
					assignment: 'Nothing'
				}
			]
		},
		alg2: {
			mostUpvotedEntryIndex: 0,
			entries: [
				{
					upvotes: 50,
					downvotes: 10,
					assignment: '19.1 summary questions'
				},
				{
					upvotes: 30,
					downvotes: 100,
					assignment: 'Nothing'
				}
			]
		},
		eng: {
			mostUpvotedEntryIndex: 0,
			entries: [
				{
					upvotes: 50,
					downvotes: 10,
					assignment: '19.1 summary questions'
				},
				{
					upvotes: 300,
					downvotes: 10,
					assignment: 'Nothing'
				}
			]
		},
		his: {
			mostUpvotedEntryIndex: 0,
			entries: [
				{
					upvotes: 100,
					downvotes: 0,
					assignment: '19.1 summary questions'
				},
				{
					upvotes: 30,
					downvotes: 100,
					assignment: 'Nothing'
				}
			]
		}
	};
	const classes = Object.keys(currentAssignments);

	const calculateLikelyhoods = (): void => {
		for (const className of classes) {
			const allEntries = currentAssignments[className].entries;
			let mostUpvotedIndex: number;
			let currentHighestRatio = 0;
			allEntries.forEach((entry, i) => {
				const ratio = entry.upvotes - entry.downvotes;
				if (ratio > currentHighestRatio) {
					currentHighestRatio = ratio;
					mostUpvotedIndex = i;
				}
			});
			currentAssignments[className].mostUpvotedEntryIndex = mostUpvotedIndex;
		}
	};

	calculateLikelyhoods();

	const upvote = (className: string, index: number): void => {
		const entry = currentAssignments[className].entries[index];
		entry.upvotes++;
		calculateLikelyhoods();
	};
	const downvote = (className: string, index: number): void => {
		const entry = currentAssignments[className].entries[index];
		entry.downvotes++;
		calculateLikelyhoods();
	};
</script>

<h1>What is the Homework Today?</h1>

{#each Object.entries(currentAssignments) as [className, data]}
	<p>
		<b>{className}:</b>
		{data.entries[data.mostUpvotedEntryIndex].assignment} (likelyhood:
		{Math.round(
			(data.entries[data.mostUpvotedEntryIndex].upvotes +
				data.entries[data.mostUpvotedEntryIndex].downvotes >
			0
				? data.entries[data.mostUpvotedEntryIndex].upvotes /
				  (data.entries[data.mostUpvotedEntryIndex].upvotes +
						data.entries[data.mostUpvotedEntryIndex].downvotes)
				: 0) * 100
		)}%; upvotes: {data.entries[data.mostUpvotedEntryIndex].upvotes}, downvotes: {data.entries[
			data.mostUpvotedEntryIndex
		].downvotes})
	</p>
	<button
		on:click={() => {
			upvote(className, data.mostUpvotedEntryIndex);
		}}>This is correct</button
	>
	<button
		on:click={() => {
			downvote(className, data.mostUpvotedEntryIndex);
		}}>This is incorrect</button
	>
{/each}
<!-- <h6>Svelte version: {VERSION}</h6> -->
