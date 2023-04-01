<script>
  // use uiuc-cis-urls package
  import { fixDocumentationURL, convertCourseURL } from "cisurls";
  import "./mirthy.css";

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

<main class="page__content">
  <ul>
    <li>
      More information can be found in the Github repo for this app at
      <a
        class="link link--gasping"
        href="https://github.com/lumirth/cisurls-web/">lumirth/cisurls-web</a
      >.
    </li>
    <li>
      The UIUC CIS API documentation can be found on
      <a
        class="link link--gasping"
        href="https://courses.illinois.edu/cisdocs/api"
        >courses.illinois.edu/cisdocs/api.</a
      >
    </li>
    <li>
      You can find a link to the an archive of what the documentation looked
      like when this app was created at
      <a
        class="link link--gasping"
        href="https://web.archive.org/web/20160517202637/courses.illinois.edu/cisdocs/api"
        >web.archive.org</a
      >.
    </li>
    <li>
      The XML schema for the UIUC CIS API can be found at
      <a
        class="link link--gasping"
        href="https://mirth.cc/cisurls-web/cisapi.xsd">/cisapi.xsd.</a
      > This schema cannot be found elsewhere, as the link in the documentation is
      broken.
    </li>
  </ul>
  <hr class="hori-line--with-margin" />
  <!-- First form for convertCourseURL -->
  <form on:submit={handleSubmitDoc}>
    <label for="originalUrl"
      >This tool converts documentation <code class="code--inline">cisapi</code> URLs to functional
      <code class="code--inline">cisapp/explorer</code> URLs:</label
    >
    <br />
    <div class="input-and-buttons-container">

      <input
      class="input-text-line"
      type="text"
      id="originalUrl"
      placeholder="ENTER DOCUMENTATION URL"
      bind:value={originalUrlDoc}
      />
      
      <button class="button" type="submit">MODIFY URL</button>
      <button class="button" type="button" on:click={handleModifyWithCascadeDoc}>
        MODIFY W/ CASCADE
      </button>
    </div>
  </form>
  {#if errMsgDoc}
    <p>{errMsgDoc}</p>
  {/if}

  {#if modifiedUrlDoc}
    <p>The modified URL is: {modifiedUrlDoc}</p>
    <button class="button"  on:click={handleCopyDoc}>
      {#if copiedDoc}
        COPIED
      {:else}
        COPY
      {/if}
    </button>
    <button class="button" type="button" on:click={handleVisitDoc}>VISIT</button>
  {/if}
  <hr class="hori-line--with-margin" />
  <!-- Second form for convertCourseURL -->
  <form on:submit={handleConvertSubmit}>
    <label for="originalUrl"
      >This tool converts <code class="code--inline">cisapp/explorer/schedule</code> course URLs to
      <code class="code--inline">search</code> URLs:</label
    >
    <br />
    <div class="input-and-buttons-container">

    <input
      class="input-text-line"
      type="text"
      id="originalUrlConv"
      placeholder="ENTER COURSE URL"
      bind:value={originalUrlConv}
    />

    <button class="button"  type="submit">CONVERT COURSE URL</button>
    </div>
  </form>
  {#if errMsgConv}
    <p>{errMsgConv}</p>
  {/if}

  {#if convertedUrlConv}
    <p>The converted URL is: {convertedUrlConv}</p>
    <button class="button"  on:click={handleCopyConv}>
      {#if copiedConv}
        COPIED
      {:else}
        COPY
      {/if}
    </button>
    <button class="button"  type="button" on:click={handleVisitConv}>VISIT</button>
  {/if}
  <hr class="hori-line--with-margin" />
  <p style="text-align:center;">
    A <a class="link link--gasping" href="https://mirth.cc">lumirth</a> tool.
  </p>
</main>

<style>
  ul {
    padding-left: 1rem;
  }
  label {
    line-height: 1;
  }
  .input-and-buttons-container {
    margin-top: 1rem;
  }
</style>
