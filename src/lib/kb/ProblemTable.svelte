<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let { ticket }: { ticket: number } = $props();

  // Counted the way the desk table counts: the one-day moves stay filed under `parser` and are listed again on their own row.
  const problemOf = (r: Report) =>
    causeOf(r, tenantOf(r));

  const merged = $derived(uniqueById(reports as Report[]).filter((r) => r.mergedInto === ticket));
  const rows = $derived(
    (['parser', 'hint', 'one-day'] as const).map((p) => {
      const rs = merged.filter((r) => (p === 'one-day' ? notes.oneDayMove.includes(r.id) : problemOf(r) === p));
      const days = rs.map((r) => r.receivedAt.slice(0, 10)).sort();
      return {
        problem: p,
        n: rs.length,
        tenants: new Set(rs.map((r) => r.tenant)).size,
        tagParser: rs.filter((r) => r.meta.intakeTag === 'parser').length,
        seen: `${days[0]} → ${days[days.length - 1]}`,
      };
    }),
  );
</script>

<p class="sum"><strong>{merged.length}</strong> reports merged into #{ticket} · <strong>{rows.length}</strong> problems</p>

<table>
  <thead><tr><th>problem</th><th>reports</th><th>tagged parser by intake</th><th>tenants</th><th>seen</th></tr></thead>
  <tbody>
    {#each rows as r (r.problem)}
      <tr><td><code>{r.problem}</code></td><td>{r.n}</td><td>{r.tagParser}</td><td>{r.tenants}</td><td>{r.seen}</td></tr>
    {/each}
  </tbody>
</table>

<style>
  .sum { font-size: 1.1rem; }
</style>
