<script>
  function modifyUrl(url, addCascade = false) {
    if (!url) {
      return "";
    }

    // Special case: if url matches https://courses.illinois.edu/cisapi/schedule/courses?year=2012&term=spring§ionTypeCode=LEC§ionTypeCode=Q&collegeCode=KV&creditHours=3&subject=CHEM&sessionId=1&gened=NAT&qp=atomic+structure
    // Then add ampersands before each section symbol and make §ionTypeCode=LEC§ionTypeCode=Q into &sectionTypeCode=LEC&sectionTypeCode=Q
    const specialUrlPattern =
      /^https:\/\/courses\.illinois\.edu\/cisapi\/schedule\/courses\?.*$/;
    if (specialUrlPattern.test(url)) {
      url = url.replace(/§/g, "&sect");
    }

    // Replace "/cisapi" with "/cisapp/explorer"
    url = url.replace("/cisapi", "/cisapp/explorer");

    // Add ".xml" before the parameters if it doesn't already exist
    if (!url.includes(".xml")) {
      const [baseUrl, queryParams] = url.split("?");
      url = queryParams ? `${baseUrl}.xml?${queryParams}` : `${baseUrl}.xml`;
    }
    // Add "mode=cascade" parameter if requested
    // Replace "mode=*" with "mode=cascade" if it already exists
    // Add "mode=cascade" with & or ? depending on if there are already parameters
    if (addCascade) {
      if (url.includes("mode=")) {
        // Replace "mode=*" with "mode=cascade" if it already exists.
        url = url.replace(/mode=[^&]*/, "mode=cascade");
      } else {
        // Add "mode=cascade" with & or ? depending on if there are already parameters.
        url += url.includes("?") ? "&mode=cascade" : "?mode=cascade";
      }
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
  <ul>
    <li>
      More information can be found in the Github repo for this app at
      <a href="https://github.com/lumirth/fix-cisapi/"
        >github.com/lumirth/fix-cisapi</a
      >.
    </li>
    <li>
      The UIUC CIS API documentation can be found on
      <a href="https://courses.illinois.edu/cisdocs/api"
        >courses.illinois.edu/cisdocs/api.</a
      >
    </li>
    <li>
      You can find a link to the an archive of what the documentation looked
      like when this app was created at
      <a
        href="https://web.archive.org/web/20160517202637/courses.illinois.edu/cisdocs/api"
        >web.archive.org/web/20160517202637/courses.illinois.edu/cisdocs/api</a
      >.
    </li>
    <li>
      The XML schema for the UIUC CIS API can be found at
      <a href="https://mirth.cc/fix-cisapi/cisapi.xsd"
        >mirth.cc/fix-cisapi/cisapi.xsd.</a
      > This schema cannot be found elsewhere, as the link in the documentation is
      broken.
    </li>
  </ul>
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

<style>
  ul {
    padding-left: 1rem;
  }
  ul li {
    margin-bottom: 0.25rem;
  }
</style>
