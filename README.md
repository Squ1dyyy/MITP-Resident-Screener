# MITP Company Shortlisting

A small data pipeline that scrapes the Moldova IT Park (MITP) residents registry, enriches each company with its public profile, and produces a filtered shortlist of active residents matching language and headcount criteria.

## Disclaimer

**This project is for educational and analytical purposes only. All data is sourced from public records on the official MITP (Moldova IT Park) website.**

## Notebook

`analize_companies.ipynb` runs the full pipeline:

1. **Setup** — imports, logging, and a frozen `Config` dataclass.
2. **Scrape registry** — fetch resident list pages from `mitp.md` (cached to `companies.csv`).
3. **Pre-filter** — keep only `Active` residents, deduplicate by id (`active_filtered_companies.csv`).
4. **HTTP client** — `requests.Session` with retry/backoff on 5xx.
5. **Schema discovery** — build a Pydantic model from a probe response.
6. **Parallel fetch** — pull each company profile with a thread pool (cached to `companies_data.csv`).
7. **Data quality** — count validation/transport errors.
8. **Normalization** — parse list-typed columns, lower-case values.
9. **EDA** — distributions by company size, programming language, and sub-industry.
10. **Shortlist** — companies using a target language (default: `python`) with `≥10` employees.
11. **Export** — write the shortlist to `filtered_companies.csv`.

## Files

| File | Purpose |
| --- | --- |
| `analize_companies.ipynb` | The pipeline notebook. |
| `companies.csv` | Raw registry list (cached). |
| `active_filtered_companies.csv` | Active residents only. |
| `companies_data.csv` | Enriched per-company profiles (cached). |
| `filtered_companies.csv` | Final shortlist output. |
| `app.log` | Per-id fetch/validation warnings. |

## Requirements

`pandas`, `pydantic`, `requests`.

## Configuration

Edit the `Config` dataclass in the notebook to change page count, worker count, target language, or cache behaviour. Set `use_cache=False` to force a fresh scrape.
