<script>
	import _ from 'lodash';

    function getCurrentDate() {
        const months = ["JAN", "FEB", "MAR", "APR", "MAY", "JUN", "JUL", "AUG", "SEP", "OCT", "NOV", "DEC"];
        const date = new Date();
        const day = date.getDate();
        const monthIndex = date.getMonth();
        const year = date.getFullYear();
        return `${day} ${months[monthIndex]} ${year}`;
    }

	let styles = [
		{ id: 'MLA', name: 'Modern Language Association' },
		{ id: 'APA', name: 'American Psychological Association' },
		{ id: 'Chicago', name: 'Chicago Manual of Style' },
		{ id: 'CSE', name: 'Council of Science Editors' }
	];
	let style = 'MLA';

	let cadet_info = {
		name: '',
		company: '',
		year: ''
	};

	let citation_info = {
		assistance_type: '',
		assistance: '',
		date: getCurrentDate(),
		location: 'West Point, NY'
	};

    /**
	 * @type {any[]}
	 */
	let formatting = [
		{
			id: 'MLA',
			name: 'Modern Language Association',
			format:
				'CDT {cadet_info.name}, {cadet_info.company} {cadet_info.year}. Assistance given to author, {citation_info.assistance_type}. {citation_info.assistance}. {citation_info.location}. {citation_info.date}.'
		},
		{
			id: 'APA',
			name: 'American Psychological Association',
			format:
				'CDT {cadet_info.name}, {cadet_info.company} {cadet_info.year}. Assistance given to author, {citation_info.assistance_type}. {citation_info.assistance}. {citation_info.location}. {citation_info.date}.'
		},
		{
			id: 'Chicago',
			name: 'Chicago Manual of Style',
			format:
				'CDT {cadet_info.name}, {cadet_info.company} {cadet_info.year}. Assistance given to author, {citation_info.assistance_type}. {citation_info.assistance}. {citation_info.location}. {citation_info.date}.'
		},
		{
			id: 'CSE',
			name: 'Council of Science Editors',
			format: 'CDT {cadet_info.name}, {cadet_info.company} {cadet_info.year}. Assistance given to author, {citation_info.assistance_type}. {citation_info.assistance}. {citation_info.location}. {citation_info.date}.'
		}
	];

	let full_citation = '';
	$: {
		let selectedFormat = _.find(formatting, { id: style });
		full_citation = selectedFormat.format;
		for (let [key, value] of Object.entries(cadet_info)) {
            if (key.includes('name')){
                // value = value.split(' ');
                // value = value.toUpperCase();
            }
			full_citation = full_citation.replace(`{cadet_info.${key}}`, value);
		}
		for (let [key, value] of Object.entries(citation_info)) {
			full_citation = full_citation.replace(`{citation_info.${key}}`, value);
		}
	}

    console.log("Go Icemen 🐻‍❄️")
</script>

<head>
	<link
		href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css"
		rel="stylesheet"
		integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD"
		crossorigin="anonymous"
	/>
	<script
		src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"
		integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN"
		crossorigin="anonymous"
	></script>
</head>
<title> Citation Generator </title>
<div class="container mt-4">
	<h1 style="display: flex; justify-content: center;">Citation Generator</h1>
	<hr />
    <style>
        span {
            width: 150px;
        }
        label {
            width: 150px;
        }

        .card {
            box-shadow: 0 2px 15px -3px rgba(0,0,0,0.07),0 10px 20px -2px rgba(0,0,0,0.04);
            border: 0;
        }

        textarea {
            width: 100%;
            border-radius: 5px;
            border: insert(0 2px 15px -3px rgba(0,0,0,0.07),0 10px 20px -2px rgba(0,0,0,0.04));

        }
    </style>
	<div class="card">
		<div class="card-body">
			<div class="input-group mb-1">
				<span class="input-group-text">Name</span>
				<input
					type="text"
					class="form-control"
					bind:value={cadet_info.name}
					placeholder="John Smith"
				/>
			</div>
			<div class="input-group mb-1">
				<span class="input-group-text">Company</span>
				<input
					type="text"
					maxlength="2"
					class="form-control"
					bind:value={cadet_info.company}
					placeholder="I3"
				/>
			</div>
			<div class="input-group mb-1">
				<span class="input-group-text">Year</span>
				<input
					type="number"
					min="2023"
					class="form-control"
					bind:value={cadet_info.year}
					placeholder="2023"
				/>
			</div>
			<div class="input-group mb-1">
				<span class="input-group-text">Assistance Type</span>
				<select class="form-select" bind:value={citation_info.assistance_type}>
					<option value="verbal discussion" selected>Verbal Discussion</option>
					<option value="written discussion">Written Discussion</option>
					<option value="written communication">Written Communication</option>
					<option value="other">Other</option>
				</select>
				<!-- {#if (citation_info.assistance == "Other")}
            <div class="input-group">
                <input type="text" class="form-control" bind:value={citation_info.assistance} placeholder="Other">
            </div>
        {/if} -->
			</div>
			<div class="input-group mb-1">
				<textarea rows="5" bind:value={citation_info.assistance} />
			</div>
			<br />
            <div class="input-group">
                <label class="input-group-text" for="inputGroupSelect01">Citation Style</label>
                <select id="inputGroupSelect01" class="form-select" bind:value={style}>
                    {#each styles as style}
                        <option value={style.id}>{style.name}</option>
                    {/each}
                </select>
            </div>
		</div>
	</div>
	<br />

    <style>
        final_citation {
            font-family: 'Times New Roman', Times, serif;
            font-size: 12pt;
        }
    </style>
    <div class="card">
        <div class="card-body">
            <final_citation>
                {full_citation}
            </final_citation>
        </div>
    </div>
	<br />
</div>
