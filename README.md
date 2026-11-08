# Davis XC portal (web)

The page template for the Davis Senior High School cross country results portal: race results, athlete histories,
championship projections and team scoring simulations for boys and girls, for the coaching staff and invited volunteers.

This repository holds code only. Athlete names and results are minors' data: they stay in a private pipeline and reach the
page only after an invited member signs in (Firebase). Nothing in this repo contains or links to that data.

- `templates/portal_template.html`: the portal. The private pipeline injects its data at `__DATA__`.
- `src/`: sign-in gate, styles, Firebase web config, home screen icons.
- `build.py`: writes `dist/index.html` with no data; GitHub Actions publishes it to Pages.
