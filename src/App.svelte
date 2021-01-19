<script>
	import Header from './components/Header.svelte';
	import Footer from './components/Footer.svelte';
	import PollList from './components/PollList.svelte';
	import CreatePollForm from './components/CreatePollForm.svelte';
	import Tabs from './shared/Tabs.svelte';

	// tabs
	let items = ['Current Polls', 'Add New Poll'];
	let activeItem = 'Current Polls';
	
	// polls
	let polls = [
		{
			id: 1,
			question: 'Python or Javascript?',
			answerA: 'Python',
			answerB: 'Javascript',
			votesA: 9,
			votesB: 15,
		},
	];

	const handleTabChange = (e) => {
		activeItem = e.detail;
	}

	const handleAdd = (e) => {
		const newPoll = e.detail;
		polls = [newPoll, ...polls];
		console.log(polls);
		activeItem = 'Current Polls';
	}

	const handleVote = (e) => {
		const { option, id } = e.detail;
		let copiedPolls = [...polls];
		let votedPoll = copiedPolls.find(poll => poll.id === id);

		if (option === 'a') {
			votedPoll.votesA++;
		}
		if (option === 'b') {
			votedPoll.votesB++;
		}

		polls = copiedPolls;
	}
</script>

<style>
	main {
		max-width: 960px;
		margin: 40px auto;
	}
</style>

<Header />
<main>
	<Tabs {activeItem} {items} on:tabChange={handleTabChange} />
	{#if activeItem === 'Current Polls'}
		<PollList {polls} on:vote={handleVote} />
	{:else if activeItem === 'Add New Poll'}
		<CreatePollForm on:add={handleAdd} />
	{/if}
</main>
<Footer />