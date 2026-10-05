<script setup>
// Data centres: IEA, Energy and AI (2025). AI: IEA 4E, Data Centre Energy Use review (2025), reference year 2023.
const rows = [
  { label: 'Data centers (% de l\'électricité mondiale)', a: '~1,5 % (415 TWh)', b: '~3 % (945 TWh)' },
  { label: 'IA (% de l\'énergie des data centers)', a: '10–15 % (2023)', b: '35–50 %' },
  { label: 'IA (énergie consommée)', a: '30–50 TWh (2023)', b: '200–400 TWh' },
]
</script>

<template>
  <div class="table-wrap card-navy">
    <div class="table-label">Part de la consommation électrique — IEA</div>
    <table>
      <thead>
        <tr>
          <th>Catégorie</th>
          <th>2024</th>
          <th>2030</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="r in rows" :key="r.label">
          <td>{{ r.label }}</td>
          <td>{{ r.a }}</td>
          <td>{{ r.b }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.table-wrap {
  padding: 0.75rem 1rem;
}

.table-label {
  font-size: 0.65rem;
  font-family: monospace;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--orange-light);
  margin-bottom: 0.5rem;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.75rem;
  color: var(--text-muted);
}

thead tr {
  border-bottom: 1px solid var(--navy-border);
}

th {
  text-align: left;
  font-weight: 600;
  color: var(--text-strong);
  padding: 0.25rem 0.5rem 0.4rem 0;
  font-size: 0.7rem;
}

td {
  padding: 0.3rem 0.5rem 0.3rem 0;
  border-bottom: 1px solid rgba(255,255,255,0.06);
}

tr:last-child td {
  border-bottom: none;
}
</style>
