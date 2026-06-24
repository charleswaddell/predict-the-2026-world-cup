---
theme: dashboard
toc: false
---

# Predict The 2026 World Cup 🚀

```js
const score = (pools, picks) =>
  pools.reduce((acc, cur, idx) => {
    const pick = picks[idx];
    console.log(cur);
    console.log(pick);
    return (
      acc +
      cur.reduce((acc, cur, idx) => (cur === pick[idx] ? acc + 1 : acc), 0)
    );
  }, 0);

const swap = (array, src, dst) =>
  array.map((_, idx, array) =>
    idx === dst ? array.at(src) : idx === src ? array.at(dst) : array.at(idx),
  );
```

```js
const pools = FileAttachment('data/world-cup-pools.csv').csv({
  typed: true,
});
```

```js
const charlesPicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-charles-parsed.csv',
).csv({
  typed: true,
});

const colinPicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-colin-parsed.csv',
).csv({
  typed: true,
});

const garrityPicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-garrity-parsed.csv',
).csv({
  typed: true,
});

const isaPicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-isa-parsed.csv',
).csv({
  typed: true,
});

const kirillPicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-kirill-parsed.csv',
).csv({
  typed: true,
});

const lubaPicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-luba-parsed.csv',
).csv({
  typed: true,
});

const mikePicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-mike-parsed.csv',
).csv({
  typed: true,
});

const reesePicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-reese-parsed.csv',
).csv({
  typed: true,
});

const wenPicks = FileAttachment(
  'data/world-cup-pool-stage-predictions-wen-parsed.csv',
).csv({
  typed: true,
});
```

```js
const createMutable = (pool) =>
  Mutable(['A', 'B', 'C', 'D'].map((idx) => pool[idx]));
const createSwap = (mutable) => (src, dst) => () => {
  mutable.value = swap(mutable.value, src, dst);
};

const mutableA = createMutable(pools[0]);
const swapA = createSwap(mutableA);

const mutableB = createMutable(pools[1]);
const swapB = createSwap(mutableB);

const mutableC = createMutable(pools[2]);
const swapC = createSwap(mutableC);

const mutableD = createMutable(pools[3]);
const swapD = createSwap(mutableD);

const mutableE = createMutable(pools[4]);
const swapE = createSwap(mutableE);

const mutableF = createMutable(pools[5]);
const swapF = createSwap(mutableF);

const mutableG = createMutable(pools[6]);
const swapG = createSwap(mutableG);

const mutableH = createMutable(pools[7]);
const swapH = createSwap(mutableH);

const mutableI = createMutable(pools[8]);
const swapI = createSwap(mutableI);

const mutableJ = createMutable(pools[9]);
const swapJ = createSwap(mutableJ);

const mutableK = createMutable(pools[10]);
const swapK = createSwap(mutableK);

const mutableL = createMutable(pools[11]);
const swapL = createSwap(mutableL);
```

```jsx
const to = (i, m) => (i == -1 ? m - 1 : i % m);

function Pool({ label, teams, swap }) {
  return (
    <div class="card">
      <h2>{label}</h2>
      <ul>
        {teams.map((item, i) => (
          <ol>
            <button onClick={swap(i, to(i + 1, teams.length))}>⬇️</button>
            {item}
            <button onClick={swap(i, to(i - 1, teams.length))}>⬆️</button>
          </ol>
        ))}
      </ul>
    </div>
  );
}
```

<div class="grid grid-cols-4">

```jsx
display(<Pool label="Pool A" teams={mutableA} swap={swapA} />);
```

```jsx
display(<Pool label="Pool B" teams={mutableB} swap={swapB} />);
```

```jsx
display(<Pool label="Pool C" teams={mutableC} swap={swapC} />);
```

```jsx
display(<Pool label="Pool D" teams={mutableD} swap={swapD} />);
```

</div>
<div class="grid grid-cols-4">

```jsx
display(<Pool label="Pool E" teams={mutableE} swap={swapE} />);
```

```jsx
display(<Pool label="Pool F" teams={mutableF} swap={swapF} />);
```

```jsx
display(<Pool label="Pool G" teams={mutableG} swap={swapG} />);
```

```jsx
display(<Pool label="Pool H" teams={mutableH} swap={swapH} />);
```

</div>
<div class="grid grid-cols-4">

```jsx
display(<Pool label="Pool I" teams={mutableI} swap={swapI} />);
```

```jsx
display(<Pool label="Pool J" teams={mutableJ} swap={swapJ} />);
```

```jsx
display(<Pool label="Pool K" teams={mutableK} swap={swapK} />);
```

```jsx
display(<Pool label="Pool L" teams={mutableL} swap={swapL} />);
```

</div>

```js
const poolsMutable = [
  mutableA,
  mutableB,
  mutableC,
  mutableD,
  mutableE,
  mutableF,
  mutableG,
  mutableH,
  mutableI,
  mutableJ,
  mutableK,
  mutableL,
];
```

```js
const convertPicks = (pick) => [pick['A'], pick['B'], pick['C'], pick['D']];

const rows = [
  ['Charles', charlesPicks],
  ['Colin', colinPicks],
  ['The Garritys', garrityPicks],
  ['Isa', isaPicks],
  ['Kirill', kirillPicks],
  ['Luba', lubaPicks],
  ['Mike', mikePicks],
  ['Reese', reesePicks],
  ['Wen', wenPicks],
].map(([name, picks]) => ({
  name,
  score: score(poolsMutable, picks.map(convertPicks)),
}));
```

```js
display(
  Inputs.table(rows, {
    sort: 'score',
    reverse: true,
    columns: ['name', 'score'],
    header: {
      name: 'Predictor',
      score: 'Score',
    },
  }),
);
```
