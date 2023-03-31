<script>
  // use uiuc-cis-urls package
  import { fixDocumentationURL, convertCourseURL } from "cisurls";

  let errMsgDoc;
  let errMsgConv;
  function modfifyDocumentationURL(url, addCascade = false) {
    // try and catch error then show error in UI
    try {
      errMsgDoc = "";
      return fixDocumentationURL(url, addCascade);
    } catch (error) {
      errMsgDoc = error.message;
      return "";
    }
  }

  let originalUrlDoc = "";
  let modifiedUrlDoc = "";
  let copiedDoc = false;
  let originalUrlConv = "";
  let convertedUrlConv = "";
  let copiedConv = false;

  function handleSubmitDoc(event) {
    event.preventDefault();
    modifiedUrlDoc = modfifyDocumentationURL(originalUrlDoc);
    copiedDoc = false; // Reset copied flag when URL is modified
  }

  function handleCopyDoc() {
    navigator.clipboard.writeText(modifiedUrlDoc);
    copiedDoc = true; // Set copied flag to display message
  }

  function handleModifyWithCascadeDoc() {
    modifiedUrlDoc = modfifyDocumentationURL(originalUrlDoc, true);
    copiedDoc = false; // Reset copied flag when URL is modified
  }

  function handleVisitDoc() {
    window.open(modifiedUrlDoc, "_blank");
  }

  async function handleConvertSubmit(event) {
    event.preventDefault();
    try {
      errMsgConv = "";
      convertedUrlConv = await convertCourseURL(originalUrlConv);
    } catch (error) {
      errMsgConv = error.message;
      convertedUrlConv = "";
    }
    copiedConv = false; // Reset copied flag when URL is converted
  }

  function handleCopyConv() {
    navigator.clipboard.writeText(convertedUrlConv);
    copiedConv = true; // Set copied flag to display message
  }

  function handleVisitConv() {
    window.open(convertedUrlConv, "_blank");
  }
</script>

<main>
  <ul>
    <li>
      More information can be found in the Github repo for this app at
      <a href="https://github.com/lumirth/cisurls-web/">lumirth/cisurls-web</a>.
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
        >web.archive.org</a
      >.
    </li>
    <li>
      The XML schema for the UIUC CIS API can be found at
      <a href="https://mirth.cc/cisurls-web/cisapi.xsd">/cisapi.xsd.</a> This schema
      cannot be found elsewhere, as the link in the documentation is broken.
    </li>
  </ul>
  <hr />
  <!-- First form for convertCourseURL -->
  <form on:submit={handleSubmitDoc}>
    <label for="originalUrl"
      >This tool converts documentation <code>cisapi</code> URLs to functional
      <code>cisapp/explorer</code> URLs:</label
    >
    <br />
    <input
      type="text"
      id="originalUrl"
      placeholder="ENTER DOCUMENTATION URL"
      bind:value={originalUrlDoc}
    />

    <button type="submit">MODIFY URL</button>
    <button type="button" on:click={handleModifyWithCascadeDoc}>
      MODIFY W/ CASCADE
    </button>
  </form>
  {#if errMsgDoc}
    <p>{errMsgDoc}</p>
  {/if}

  {#if modifiedUrlDoc}
    <p>The modified URL is: {modifiedUrlDoc}</p>
    <button on:click={handleCopyDoc}>
      {#if copiedDoc}
        COPIED
      {:else}
        COPY
      {/if}
    </button>
    <button type="button" on:click={handleVisitDoc}>VISIT</button>
  {/if}
  <hr />
  <!-- Second form for convertCourseURL -->
  <form on:submit={handleConvertSubmit}>
    <label for="originalUrl"
      >This tool converts <code>cisapp/explorer/schedule</code> course URLs to
      <code>search</code> URLs:</label
    >
    <br />
    <input
      type="text"
      id="originalUrlConv"
      placeholder="ENTER COURSE URL"
      bind:value={originalUrlConv}
    />

    <button type="submit">CONVERT COURSE URL</button>
  </form>
  {#if errMsgConv}
    <p>{errMsgConv}</p>
  {/if}

  {#if convertedUrlConv}
    <p>The converted URL is: {convertedUrlConv}</p>
    <button on:click={handleCopyConv}>
      {#if copiedConv}
        COPIED
      {:else}
        COPY
      {/if}
    </button>
    <button type="button" on:click={handleVisitConv}>VISIT</button>
  {/if}
  <hr />
  <p style="text-align:center;">
    A <a href="https://mirth.cc">lumirth</a> tool.
  </p>
</main>

<style>
  /* google inter font */
  @import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap");
  /* google fira code font */
  @import url("https://fonts.googleapis.com/css2?family=Fira+Code&display=swap");
  main {
    margin: 1rem;
    font-family: "Inter", sans-serif;
    /* max-width and centering */
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
    margin-top: 4rem;
    margin-bottom: 4rem;
  }

  @media (max-width: 800px) {
    main {
      margin-top: 1rem;
      margin-bottom: 1rem;
    }

    button {
      height: 2rem; 
      margin-top: 1rem; 
    }
    input {
      height: 1.7rem;
      outline: none;
      border: 1px solid transparent;
    }
    label {
      line-height: 1.75;
    }
    label code {
      line-height: 1.75;
    }
  }
  code {
    background-color: #f8f8f8;
    border: 1px solid #e7e7e7;
    border-radius: 0.5rem;
    padding: 0.1rem 0.3rem;
  }

  button {
    border-radius: 0.2rem;
    cursor: pointer;
    margin-left: 0.1rem;
    margin-right: 0.1rem;
    border: 1px solid #e7e7e7;
    /* border-radius: 0.2rem; */
    padding: 0.1rem 0.3rem;
    background-color: #e7e7e7;
    font-family: "Inter", sans-serif;
    color: black;
    font-weight: 400;
    transition: all 0.1s ease-in-out;
  }
  button:hover {
    border: 1px solid #000;
    transition: all 0.1s ease-in-out;
  }
  input {
    font-family: 'Inter', sans-serif;
    background-color: #e7e7e7;
    border-radius: 0.2rem;
    width: 225px;
    margin-top: 1rem;
    outline: none;
    border: 1px solid transparent;
    padding: 0.1rem 0.3rem;
  }
  input::placeholder {
    color: #000;
    opacity: 0.3;
  }
  input:focus::placeholder {
    color: transparent;
    transition: all 1s ease-in-out;
  }
  input:focus {
    border: 1px solid #000;
    transition: all 0.1s ease-in-out;
  }
  a {
    color: #000;
    text-decoration: underline;
    text-underline-offset: 0.15rem;
    text-decoration-thickness: 0.075rem;
    transition: all 0.2s ease-in-out;
  }
  a:hover {
    text-decoration: underline;
    text-underline-offset: 0.35rem;
    text-decoration-thickness: 0.075rem;
    color: #ff0000;
    transition: all 0.2s ease-in-out;
  }
  hr {
    margin-top: 2rem;
    margin-bottom: 2rem;
    border: 0;
    border-top-color: currentcolor;
    border-top-style: none;
    border-top-width: 0px;
    border-top: 1px solid #e7e7e7;
    margin: 2rem 0;
  }
  ul {
    padding-left: 1rem;
  }
  ul li {
    margin-bottom: 0.5rem;
  }

  code {
    position: relative;
    bottom: 0.1rem;
  }
</style>
