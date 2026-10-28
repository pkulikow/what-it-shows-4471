# what-it-shows-4471

MEGA.dev S01E03. Ticket #4471 merged 412 date reports as one bug. It holds three problems.

- Issues 1 to 3: one ticket per problem, each with its report count (374, 33, 5).
- `src/routes/kb/incidents/4471-three-problems/+page.svx`: the kb page, signed `pkulikow`.
- `src/lib/kb/ProblemTable.svelte`: computes the table on the page from the ticket data.
- `rendered/4471-three-problems.html`: the page as the desk sees it.
