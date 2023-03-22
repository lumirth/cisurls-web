<script>
	function modifyUrl(url, addCascade = false) {
	  if (!url) {
		return "";
	  }
  
	  // Replace "/cisapi" with "/cisapp/explorer"
	  url = url.replace("/cisapi", "/cisapp/explorer");
  
	  // Add ".xml" before the parameters
	  const index = url.indexOf("?");
	  if (index !== -1) {
		url = url.slice(0, index) + ".xml" + url.slice(index);
	  } else {
		url += ".xml";
	  }
  
	  // Add "mode=cascade" parameter if requested
	  if (addCascade) {
		url += (index !== -1 ? "&" : "?") + "mode=cascade";
	  }
  
	  return url;
	}
  
	let originalUrl = "";
	let modifiedUrl = "";
	let copied = false;
  
	function handleSubmit(event) {
	  event.preventDefault();
	  modifiedUrl = modifyUrl(originalUrl);
	  copied = false; // Reset copied flag when URL is modified
	}
  
	function handleCopy() {
	  navigator.clipboard.writeText(modifiedUrl);
	  copied = true; // Set copied flag to display message
	}
  
	function handleModifyWithCascade() {
	  modifiedUrl = modifyUrl(originalUrl, true);
	  copied = false; // Reset copied flag when URL is modified
	}
  
	function handleVisit() {
	  window.open(modifiedUrl, "_blank");
	}
  </script>
  
  <main>
	The CIS API documentation can be found on <a
	  href="https://courses.illinois.edu/cisdocs/api">this page.</a
	>
	<br />
	You can find a link to the an archive of what the documentation looked like when this app was created <a href="https://web.archive.org/web/20160517202637/http://courses.illinois.edu/cisdocs/api">here</a>.
	<br />
	<br />
	<form on:submit={handleSubmit}>
	  <label for="originalUrl">Enter URL:</label>
	  <input type="text" id="originalUrl" bind:value={originalUrl} />
  
	  <button type="submit">MODIFY URL</button>
	  <button type="button" on:click={handleModifyWithCascade}>
		MODIFY W/ CASCADE
	  </button>
	</form>
  
	{#if modifiedUrl}
	  <p>The modified URL is: {modifiedUrl}</p>
	  <button on:click={handleCopy}>
		{#if copied}
		  COPIED
		{:else}
		  COPY
		{/if}
	  </button>
	  <button type="button" on:click={handleVisit}>VISIT</button>
	{/if}
  </main>
  