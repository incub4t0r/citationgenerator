<script>
	import _ from 'lodash';

    function getCurrentDate() {
        const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        const date = new Date();
        const day = date.getDate();
        const monthIndex = date.getMonth();
        const year = date.getFullYear();
        return `${day} ${months[monthIndex]} ${year}`;
    }

    const ranks = [
        {id: "CDT", name: "Cadet"},
        {id: "Dr", name: "Doctor"},
        {id: "LT", name: "LT"},
        {id: "CPT", name: "CPT"},
        {id: "MAJ", name: "MAJ"},
        {id: "LTC", name: "LTC"},
        {id: "COL", name: "COL"},
        {id: "BG", name: "BG"},
        {id: "MG", name: "MG"},
        {id: "LTG", name: "LTG"},
    ]

	const styles = [
		{ id: 'MLA', name: 'Modern Language Association' },
		{ id: 'APA', name: 'American Psychological Association' },
		{ id: 'Chicago', name: 'Chicago Manual of Style' },
		{ id: 'CSE', name: 'Council of Science Editors' }
	];

	let style = 'MLA';

	let citer_info = {
        rank: '',
		name: '',
		company: '',
		year: '',
        format_name: ''
	};

	let citation_info = {
		assistance_type: '',
		assistance: '',
		date: getCurrentDate(),
		location: 'West Point, NY'
	};

    let preparse_info = {
        name: '',
        company: '',
        year: '',
    }

	let full_citation = '';

    /**
	 * @param {string} str
	 */
    function apa_name(str) {
        if (str == null || str.length == 0) return "";
        let names = str.split(' ');
        let lastName = names[names.length - 1];
        let firstInitial = names[0][0];
        return `${lastName}, ${firstInitial}`;
    }

    /**
	 * @param {string} str
	 */
    function cse_name(str) {
        if (str == null || str.length == 0) return "";
        let names = str.split(' ');
        let lastName = names[names.length - 1];
        return `${lastName}, IB`;
    }

    /**
	 * @param {string} str
	 */
    function format_name(str) {
        if (str == null || str.length == 0) return "";
        return str.split(" ").reverse().join(", ");
    }

    /**
     * @param {string} str
     */
    function mla_date(str) {
        if (str == null || str.length == 0) return "";
        return str.split(" ")[0] + " " + str.split(" ")[1].slice(0,3) + ". " + str.split(" ")[2];
    }

    /**
     * @param {string} str
     */
    function apa_date(str) {
        if (str == null || str.length == 0) return "";
        return "(" + str.split(" ")[2] + ")";
    }


    /**
     * @param {string} str
     */
    function cse_date(str) {
        if (str == null || str.length == 0) return "";
        return str.split(" ")[2] + " " + str.split(" ")[1].slice(0,3) + " " + str.split(" ")[0];
    }
    $: {
        citer_info.company = preparse_info.company.split("").join("-");
        citer_info.year = (() => {
            if (preparse_info.year == null || preparse_info.year.toString().length == 0){
                return "";
            }
            else if (preparse_info.year.toString().length >= 4) {
                return ("'" + preparse_info.year.toString().slice(2,4))
            }
            else if (preparse_info.year.toString().length == 3) {
                return ("'" + preparse_info.year.toString().slice(1,3))
            }
            else {
                return ("'" + preparse_info.year)
            }
        })();
    }
    $: formatted_author = style === "APA" ? apa_name(preparse_info.name) : 
                                    // "CSE" ? cse_name(preparse_info.name) :
                                    format_name(preparse_info.name);
    $: formatted_date = style === "MLA" ? mla_date(citation_info.date) : 
                                    "APA" ? apa_date(citation_info.date) :
                                    "Chicago" ? citation_info.date :
                                    "CSE" ? cse_date(citation_info.date):
                                    citation_info.date;

    $: mla = `${formatted_author} ${citer_info.rank} ${citer_info.company} ${citer_info.year}. Assistance given to author, ${citation_info.assistance_type}. ${citation_info.assistance}. ${citation_info.location}. ${formatted_date}.`;
    $: apa = `${formatted_author} ${citer_info.rank} ${citer_info.company} ${citer_info.year}. ${formatted_date}. Assistance given to author, ${citation_info.assistance_type}. ${citation_info.assistance}. ${citation_info.location}.`;
    $: chicago = `${formatted_author} ${citer_info.rank} ${citer_info.company} ${citer_info.year}. Assistance given to author, ${citation_info.assistance_type}. ${citation_info.assistance}. ${citation_info.location}. ${citation_info.date}.`;
    $: cse = `${formatted_author} ${citer_info.rank} ${citer_info.company} ${citer_info.year}. ${formatted_date}. Assistance given to author, ${citation_info.assistance_type}. ${citation_info.assistance}. ${citation_info.location}.`;
    $: full_citation = style == 'MLA' ? mla : style == 'APA' ? apa : style == 'Chicago' ? chicago : cse;


    let copytext = "Copy to clipboard";
    /**
	 * @param {string} text
	 */
    function copyToClipboard(text) {
        navigator.clipboard.writeText(text).then(function() {
            copytext = "Copied!";
            setTimeout(() => {
                copytext = "Copy to clipboard";
            }, 1000);
            console.log('Copying to clipboard was successful!');
        }, function(err) {
            console.error('Could not copy text: ', err);
        });
    }

    function reset() {
        citer_info = {
            rank: '',
            name: '',
            company: '',
            year: '',
            format_name: ''
        };
        citation_info = {
            assistance_type: '',
            assistance: '',
            date: getCurrentDate(),
            location: 'West Point, NY'
        };
        preparse_info = {
            name: '',
            company: '',
            year: '',
        }
        full_citation = '';
    }
    console.log("Go Icemen 🐻‍❄️")
</script>


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

    input {
        box-sizing: border-box;
        -webkit-user-select: auto !important;
        -moz-user-select: auto !important;
        -ms-user-select: auto !important;
        user-select: auto !important;
        }

    select {
    box-sizing: border-box;
    -webkit-user-select: auto !important;
    -moz-user-select: auto !important;
    -ms-user-select: auto !important;
    user-select: auto !important;
    }
    final_citation {
        font-family: 'Times New Roman', Times, serif;
        font-size: 12pt;
        /* make it look like a textarea */
        display: block;
    }
</style>

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
	<div class="card">
		<div class="card-body">
            <div class="input-group mb-1">
				<span class="input-group-text">Rank</span>
                <select class="form-select" bind:value={citer_info.rank}>
                    {#each ranks as rank}
                        <option value={rank.id}>{rank.name}</option>
                    {/each}
				</select>
				
			</div>
			<div class="input-group mb-1">
				<span class="input-group-text">Name</span>
				<input
					type="text"
					class="form-control"
					bind:value={preparse_info.name}
					placeholder="John Smith"
				/>
			</div>
			<div class="input-group mb-1">
				<span class="input-group-text">Company</span>
				<input
					type="text"
					maxlength="2"
					class="form-control"
					bind:value={preparse_info.company}
					placeholder="I3"
				/>
			</div>
			<div class="input-group mb-1">
				<span class="input-group-text">Year</span>
				<input
					type="number"
					min="2023"
					class="form-control"
					bind:value={preparse_info.year}
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
				{#if (citation_info.assistance == "Other")}
                    <div class="input-group">
                        <input type="text" class="form-control" bind:value={citation_info.assistance} placeholder="Other">
                    </div>
                {/if}
			</div>
			<div class="input-group mb-1">
				<textarea rows="5" bind:value={citation_info.assistance} />
			</div>
            <!-- <div class="input-group mb-1">
				<input type="text" class="datepicker">
			</div> -->
			<br />
            <div class="input-group mb-4">
                <label class="input-group-text" for="inputGroupSelect01">Citation Style</label>
                <select id="inputGroupSelect01" class="form-select" bind:value={style}>
                    {#each styles as style}
                        <option value={style.id}>{style.name}</option>
                    {/each}
                </select>
            </div>
            <div class="input-group" >
                <button class="btn btn-primary form-control" on:click={()=>copyToClipboard(full_citation)}>{copytext}</button>
                <button class="btn btn-warning form-control" on:click={()=>reset()}>Clear</button>
            </div>
		</div>
	</div>
	<br />
    <div class="card mb-4">
        <div class="card-body">
            <final_citation>
                {full_citation}
            </final_citation>
        </div>
    </div>
    <div class="card mb-4">
        <div class="card-body">
            <a href="https://www.westpoint.edu/sites/default/files/pdfs/ABOUT/Student%20Consumer%20Info/daw-june2011.pdf">Citation style guide</a>
        </div>
    </div>
</div>
