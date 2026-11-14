# Rota do Excedente (Surplus Route)

[Português](README.md) · **English**

**Demo:** https://gabriela-doria.github.io/rota-do-excedente/

> Food going to waste on one side and missing on the other is, first of all, a **logistics** problem.

A prototype that groups restaurants with surplus food into routes to community kitchens, so one courier can run each route during the **off-peak window** between lunch and dinner (3–6 pm).

## Why it exists

I developed this idea for an AI challenge (Oct 2026) and then turned it into a working prototype.

- **24.2%** of Brazilian households were food insecure in 2024 ([IBGE](https://agenciadenoticias.ibge.gov.br/pt/agencia-noticias/44728-mais-de-dois-milhoes-de-lares-saem-da-inseguranca-alimentar-em-2024.html)).
- **28%** of global food waste happens in *food service*: restaurants, bakeries and similar ([UNEP, Food Waste Index 2024](https://www.unep.org/pt-br/noticias-e-reportagens/comunicado-de-imprensa/mundo-joga-fora-mais-de-1-bilhao-de-refeicoes-por-dia)).
- Donation initiatives already exist. My thesis: what community kitchens lack is **transport**. A delivery platform has couriers on the street and, **as a hypothesis**, idle capacity in the off-peak window.

## How it works

1. By 3 pm, each restaurant reports "I have surplus today" and how many kg.
2. The algorithm groups 2 to 8 nearby restaurants into a route to the nearest community kitchen.
3. A courier runs the route off-peak and is paid per route.
4. The dashboard shows kg rescued, km, cost per kg and whether the route pays off.

### Algorithm

| Step | Method |
|---|---|
| Assignment | Each restaurant goes to the nearest kitchen. |
| Clustering | *Sweep*: sorts restaurants by angle around the kitchen and closes a route when it hits capacity (kg) or the stop limit. |
| Stop order | Nearest neighbor, refined with *2-opt*. The route ends at the kitchen. |
| Distance | Haversine × 1.3 (street factor). |
| Time | Travel at average speed + minutes per stop; warning if it goes past the off-peak window. |
| Stop criterion | If the route's **cost per kg** exceeds what the kitchen would pay to **buy** the food, the route doesn't pay off. |

Everything runs in the browser, in a single `index.html` with [Leaflet](https://leafletjs.com/) and OpenStreetMap. No backend and no real data.

## Assumptions (all hypotheses to validate)

| Assumption | Default |
|---|---|
| Capacity per route | 30 kg |
| Max stops per route | 8 |
| Pay per route | R$ 35 |
| Kitchen's cost to buy the food | R$ 12/kg |
| Average speed | 18 km/h |
| Time per stop | 5 min |
| kg per meal | 0.4 kg |

All are editable on the page. The restaurants and kitchens in the example are **fictitious**.

## The pilot I would propose

- **4 weeks**, 1 region of São Paulo, 20 restaurants (buffets and bakeries), 2 community kitchens, 2 couriers.
- **Manual first:** form + spreadsheet + this algorithm building the routes.
- **KPIs:** kg rescued per week · % of restaurants reporting 3+ times a week · cost per kg · courier earnings per off-peak hour.
- **Risks:** food safety (Brazil's Law 14,016/2020 protects donors when the food is fit for consumption; protocol: only food that never reached the counter, packed and labeled), restaurant adoption and route cost (which is what the pilot measures).

## Next steps

- [ ] Geocode addresses instead of clicking on the map.
- [ ] Use real road routes (OSRM) instead of straight line × factor.
- [ ] Phase 2 with AI: forecast, from history, how much each restaurant will have left over and suggest how much to produce. Prevention comes before redistribution.

## How I built it

Built with AI from the case I presented: Claude as the coding agent, with the logic, assumptions and criteria defined by me.

---

Made by [Gabriela Dória](https://github.com/gabriela-doria) · [MIT](LICENSE) license
