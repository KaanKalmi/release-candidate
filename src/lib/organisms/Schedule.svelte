<script>
  import Heading from "$lib/molecules/Heading.svelte";
  import { prettyDate, longDate } from "$lib/utils/date.js";
  let { weekPlans } = $props();
  const checkKeywords = (content) => {
    const strings = [
      "sprintplanning",
      "kickoff",
      "workshop",
      "opdrachtgever",
      "code review",
      "retrospect ",
      "wrap-up",
      "review",
    ];
    let newString = "";
    if (content) {
      const splitted = content.split("<p>");
      splitted.forEach((paragraph) => {
        let toReplace = strings.filter((s) => {
          if (paragraph.toLowerCase().includes(s)) {
            return s
              .replace("we ♥ web", "we love web", s)
              .replaceAll(" ", "-", s);
          }
        });
        let classes = toReplace.join(" ");
        paragraph = "<p>" + paragraph;
        newString += paragraph.replace(
          "<p>",
          `<p class="${classes}">`,
          paragraph,
        );
      });
    }
    return newString;
  };

  const plans = [];
  weekPlans.forEach((plan) => {
    let startDate = new Date(plan["mondayDate"]);
    const week = [
      {
        date: startDate,
        weekDay: "Maandag",
        content: checkKeywords(plan["monday"].html),
      },
      {
        date: addDays(startDate, 1),
        weekDay: "Dinsdag",
        content: checkKeywords(plan["tuesday"].html),
      },
      {
        date: addDays(startDate, 2),
        weekDay: "Woensdag",
        content: checkKeywords(plan["wednesday"].html),
      },
      {
        date: addDays(startDate, 3),
        weekDay: "Donderdag",
        content: checkKeywords(plan["thursday"].html),
      },
      {
        date: addDays(startDate, 4),
        weekDay: "Vrijdag",
        content: checkKeywords(plan["friday"].html),
      },
    ];
    plans.push(week);
  });

  function addDays(date, days) {
    var result = new Date(date);
    result.setDate(result.getDate() + days);
    return result;
  }
</script>

{#if weekPlans && weekPlans.length > 0}
  <section>
    <Heading title="Sprint planning" />
    {#each plans as week, i}
      <div class="week-container">
        <h3>Week {i + 1}</h3>
        <div class="week">
          {#each week as day, i}
            <div class="day day--{day.weekDay}">
              <h4 class="weekday">
                {day.weekDay} <span> {prettyDate(day.date)} </span>
              </h4>
              <div data-label="{day.weekDay} {prettyDate(day.date)}">
                {@html day.content}
              </div>
            </div>
          {/each}
        </div>
      </div>
    {/each}
  </section>
{/if}

<style>
  section {
    padding-block: 2em;
    background: var(--grey);
  }

  section > :global(h2) {
    padding: 0;
    margin: 0.5em 0 0 0;
  }

  .week-container {
    padding: 1rem 0 1rem 0;
  }

  .week + .week {
    margin-top: 2rem;
  }

  h3 {
    font-weight: normal;
    margin-top: 0;
    position: sticky;
    background: var(--grey);
    top: 0;
    z-index: 1;
    margin-left: -.2em;
    padding-left: .2em;
    padding-block: .5em;
    margin-block: 0;
  }

  .week {
    display: flex;
    flex-direction: column;
    gap: 1em;
  }

  .day {
    border: 2px solid var(--turquoise);
    border-radius: var(--rounded);
    padding: 1rem;
    background: var(--white);
    box-shadow: -4px 4px var(--lavender);
    word-wrap: break-word;
    overflow-wrap: break-word;
    word-break: break-word;
  }

  .weekday {
    font-size: 1.125rem;
    margin: 0 0 0.5rem;
    text-transform: uppercase;
    color: var(--blueberry);
  }

  .day :global(p) {
    --border: var(--lavender);
    position: relative;
    font-size: 1rem;
    padding-left: 1rem;
  }

  .day :global(p::before) {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    border-radius: 1rem;
    width: 4px;
    height: 100%;
    background-color: var(--border);
    opacity: 1;
  }

  .day :global(.sprintplanning) { 
    --border: var(--lavender); 
  }

  .day :global(.workshop) { 
    --border: var(--turquoise); 
  }

  .day :global(.opdrachtgever), .day :global(.code-review), .day :global(.review), .day :global(.retrospect), .day :global(.kickoff), .day :global(.wrap-up) { 
    --border: var(--call-to-action); 
  }

  @media (630px <= width) {
    .week {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 1.5em;
    }

    .day {
      height: auto;
      max-height: 300px;
      overflow-y: auto;
      scrollbar-color: var(--lavender) #e0e0e0;
      scrollbar-width: thin;
    }
  }
</style>
