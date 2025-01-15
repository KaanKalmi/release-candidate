<script>
  import { onMount } from "svelte";
  import Heading from "$lib/molecules/Heading.svelte";
  import SprintLink from "$lib/molecules/SprintLink.svelte";
  let { semester } = $props();

  onMount(() => {
    // Create a new IntersectionObserver instance
    const observer = new IntersectionObserver(
      (entries) => {
        // For each entry (element being observed)
        entries.forEach((entry) => {
          // Toggle the "focused" class based on whether the element is intersecting
          entry.target.classList.toggle("focused", entry.isIntersecting);
        });
      },
      {
        // Observer options
        root: null, // Use the viewport as the root
        rootMargin: "0px", // No margin around the root
        threshold: [0, 0.5], // Trigger when 50% of the element is visible
      },
    );

    document
      .querySelectorAll(".semester")
      .forEach((semester) => observer.observe(semester));
  });
</script>

<section class="semester">
  <a href="/{semester.slug}">
    <Heading title={`Semester`} subtitle={semester.title} />
  </a>
  <ol>
    {#each semester.sprints as sprint, index}
      <SprintLink
        {semester}
        {sprint}
        nextSprint={index !== semester.sprints.length - 1
          ? semester.sprints[index + 1]
          : false}
      />
    {/each}
  </ol>
</section>

<style>
  .semester {
    padding: 0;
    border: none;
    scroll-snap-align: start;
    margin-left: -.15em;
    transition: filter 0.3s ease-in-out;
  }

  .semester:not(.focused) {
    filter: blur(5px);
  }

  .semester a {
    color: var(--blueberry);
    text-decoration: none;
    display: inline-block;
    font-size: 1.5em;
    margin: 0 -0.5rem 0.5rem;
  }

  .semester a:focus {
    color: var(--blueberry);
  }

  ol {
    list-style: none;
    padding: 0.5rem;
    margin-top: -1.5em;
    margin-left: -.25em;
    width: 320px;
  }

  :global(.semester h2 ) {
    line-height: 1.1;
    font-size: 1.25rem !important;
  }

  :global(.semester h2 > span) {
    font-size: 0.6em;
    letter-spacing: 0;
    text-transform: uppercase;
  }

  @media (min-width: 960px) {
    .semester > a {
      color: var(--blueberry);
      text-decoration: none;
      display: inline-block;
    }

    .semester > a:focus {
      color: var(--blueberry);
    }

    ol {
      list-style: none;
      width: 360px;
    }
  }
</style>
