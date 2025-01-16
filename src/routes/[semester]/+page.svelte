<script>
  import Heading from "$lib/molecules/Heading.svelte";
  import Content from "$lib/organisms/ContentSemester.svelte";
  import SprintLink from "$lib/molecules/SprintLink.svelte";

  let { data } = $props();

  const semester = data;
</script>

<Heading title="Semester" subtitle={semester.title}/>
<section>
  <div class="content-container">
    <Content {semester} />
  </div>
    <article>
      <h3>Sprints</h3>
      <ol>
        {#each semester.sprints as sprint}
          <SprintLink {semester} {sprint}/>
        {/each}
      </ol>
    </article>
</section>

<style>
  :global(h2) { font-size: 1.25em !important; padding-top: 1.5em !important;}
  section{
    display: flex;
    flex-direction: column;

    .content-container{ width: 100%; }
    article{
      ol{ padding-left: 0; }
    }
  }

  @media (750px <=width){
    :global(h2) { padding-top: 2.5em !important;}
    section{
      display: flex;
      flex-direction: column;

      .content-container{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
      }
    }
  }

  @media (1024px <= width){
    section{
      display: grid;
      grid-template-areas: "content sprints";
      grid-template-columns: 2fr 1fr;

      .content-container{ grid-area: content; }
      
      article{
        padding-left: 3em;
        padding-bottom: 1em;
        ol{ grid-area: sprints; }
      }
    }
  }
</style>