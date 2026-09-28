# Did oil slicks increase after the Santa Ynez platforms restarted?

In 2025, Sable Offshore restarted oil production at three offshore platforms in the Santa Barbara Channel: **Harmony, Heritage and Hondo**. They had been shut down since the 2015 Refugio pipeline spill. This project uses satellite images of the ocean to ask a simple question: **did oil slicks near those platforms become more common after the restart?**

## 📄 Read the full report

**➜ [gmcdonald-sfg.github.io/sb-channel-oil-slicks](https://gmcdonald-sfg.github.io/sb-channel-oil-slicks/)**

The report has the maps, charts, tables and full explanation. It opens in any web browser; you don't need to install anything. It is rebuilt automatically every time the analysis or data change, so the link always shows the latest version.

---

## The short answer

**Maybe, but we can't be sure yet.**

Slicks did become more common near two of the platforms (Harmony and Hondo) after the restart. But slicks also became more common along the nearby coastline, where oil naturally seeps out of the seafloor. So far, the data can't tell those two explanations apart.

## What we found

*Based on satellite data from January 2023 to September 2026. The report always has the latest numbers.*

1. **The satellites started looking more often at the same time as the restart.** A new European radar satellite (Sentinel-1C) began operating in spring 2025, just weeks before the restart. More photos means more slicks get spotted everywhere, whether or not anything changed on the water. To avoid being fooled by this, we don't count slicks. Instead, for each satellite pass we measure **what share of the ocean it photographed was covered by a slick**.

2. **The area around the platforms did get oilier.** Within 5 km (about 3 miles) of the platforms, slicks covered **0.56%** of the ocean surface in a typical satellite image before the restart and **1.53%** after, nearly three times as much. In the rest of the Channel, the change was much smaller: from 0.72% to 0.89%.

3. **The increase is at Harmony and Hondo, not Heritage.**

   | Platform | Satellite passes that saw oil nearby: **before** | **after** |
   |---|---|---|
   | Harmony | 4% | 10–15% |
   | Hondo | 8% | 17–19% |
   | Heritage | 5% | 4% (no change, even after Heritage restarted in April 2026) |

4. **But the increase is not strong enough to be sure it's real.** Slicks are more common in summer and fall, and more of the "after" period falls in those seasons. After accounting for that, the increase is smaller and falls within the range that random variation could produce. As a test, we placed make-believe "platforms" at 260 other spots in the Channel. About **1 in 12** of those fake locations showed an increase at least as big as the real platforms, just by chance.

5. **The nearby coastline got oilier too.** The Channel has well-known natural oil seeps along the coast from Coal Oil Point to Carpinteria, just east of the platforms. That stretch saw about the same increase as the platform area. Ocean currents along this coast usually flow west, which could carry seep oil past the platforms.

**Bottom line:** the data fit a modest increase in slicks around Harmony and Hondo after the restart. They fit a coast-wide increase in natural seep oil drifting past the platforms about equally well. More data, especially once Hondo is fully producing, should help tell the two apart.

## How the analysis works, in plain English

- **Where the data come from.** [SkyTruth Cerulean](https://cerulean.skytruth.org) scans every image from the European Space Agency's Sentinel-1 radar satellites and uses machine learning to outline oil slicks. On radar, oil shows up as smooth dark patches because it calms the small waves on the sea surface. Cerulean's data are free and public.
- **Before vs. after, near vs. far.** We compare the platform area (within 5 km) with the rest of the Channel (more than 10 km away), before and after the restart. If the platforms caused more slicks, the platform area should have changed *more* than the rest of the Channel. Scientists call this a **before/after/control/impact (BACI)** design.
- **Only counting what the satellite actually saw.** The ocean is divided into a grid of 1 km squares. For every satellite pass, we note which squares were photographed and which had oil. This keeps extra satellite passes from inflating the results.
- **Stress tests.** We checked whether the result holds up when we change the size of the platform zone, compare against different parts of the coast, pretend the restart happened a year earlier, leave out slicks labeled as natural seeps, and move the platforms to fake locations (described above).

## Important caveats

- **Radar can't tell a seep slick from a platform slick.** Both look the same from space. Cerulean's "natural" and "vessel" labels are computer guesses, and very few slicks have been checked by a person.
- **Wind matters.** Radar only sees slicks in light to moderate wind. We don't yet adjust for local wind.
- **Only one "treated" area.** There is just one set of platforms to study, so ordinary statistics are less reliable. That's why we lean on the fake-platform test.
- **The "after" period is short:** about a year and a half so far.

---

## Updating the report

You **don't** need to be a programmer to make simple changes. The whole project lives in a single file, **[`report.qmd`](report.qmd)**. It holds the settings, the analysis and the written report. When you save a change to it on GitHub, the website updates by itself in about 10 minutes.

### Changing a setting (for example, the restart date or study dates)

1. Open [`report.qmd`](report.qmd) on GitHub and click the ✏️ pencil icon (*Edit this file*).
2. Scroll to the section headed **`SETTINGS`** near the top. Every setting has a plain-English comment above it. For example:
   ```r
   restart  <- as.POSIXct("2025-05-01", tz = "UTC")  # Harmony test restart: start of "After"
   R_imp    <- 5     # km. Impact zone = ocean within this distance of any platform
   ```
3. Change the value, keeping the quotation marks and commas as they are.
4. Click **Commit changes**.
5. Watch the **Actions** tab at the top of the repo. A green ✓ means the new report is live at the link above. A red ✗ means something broke. Click it to see the error, or undo your change.

### Getting the newest satellite data

The report uses a saved copy of the Cerulean data (in the `data/` folder), so results don't shift unexpectedly. To pull in newer detections:

1. In the settings, change `refresh_data <- FALSE` to `refresh_data <- TRUE`.
2. Render the report on your computer (see below). This downloads fresh data into `data/`.
3. Change the setting back to `FALSE`, then commit both `report.qmd` and the updated `data/` folder.

Also reread the written interpretation in the report. The numbers update themselves, but the sentences describing them were written for the September 2026 data.

### Common tasks

| I want to… | Do this |
|---|---|
| Read the latest results | Open the [report link](https://gmcdonald-sfg.github.io/sb-channel-oil-slicks/) |
| See how a number was calculated | In the report, click **Show code** under that section |
| Change dates, platforms or zone sizes | Edit the `SETTINGS` section of `report.qmd` (steps above) |
| Add new satellite data | Set `refresh_data <- TRUE` and render on your computer (steps above) |
| Edit the written text | Edit the text in `report.qmd` directly. It's ordinary text between the code sections |
| Rebuild the website by hand | **Actions** tab → *Render and publish report* → **Run workflow** |
| Download the HTML file itself | **Actions** tab → latest run → *report-html* under Artifacts |

### Running it on your own computer

You need:

- **[R](https://cran.r-project.org/)**, the statistics program the analysis is written in.
- **[Quarto](https://quarto.org/docs/get-started/)**, which turns `report.qmd` into a web page. It comes bundled with [RStudio](https://posit.co/download/rstudio-desktop/), a friendly editor for R.

One time only, install the R packages the report uses by running this in R:

```r
install.packages(c("tidyverse", "data.table", "sf", "jsonlite", "patchwork",
                   "knitr", "rmarkdown", "kableExtra", "rnaturalearth", "rnaturalearthdata"))
```

Then build the report, either by opening `report.qmd` in RStudio and clicking **Render**, or from a terminal in this folder:

```sh
quarto render report.qmd
```

It takes about 2 minutes and creates `report.html`, which you can open in any browser.

## What's in this folder

| File or folder | What it is |
|---|---|
| [`report.qmd`](report.qmd) | **The whole project:** settings, data download, analysis and the written report, in one file |
| `data/` | Saved copy of the Cerulean data: 1,235 slick outlines (`slicks.csv`) and every satellite image footprint (`s1_scenes.json`) |
| `.github/workflows/render-report.yml` | Instructions GitHub follows to rebuild and publish the report automatically |
| `report.html` | The finished report, created when you render (not stored in the repo; the live copy is on the website) |

## Glossary

- **Slick:** a patch of oil on the sea surface, as outlined by Cerulean's software.
- **Sentinel-1:** European Space Agency radar satellites that photograph the ocean day or night, through clouds.
- **Pass:** one trip of a satellite over the Channel, which produces one set of images.
- **Impact zone:** the ocean within 5 km of any of the three platforms.
- **Control zone:** the rest of the Channel, more than 10 km from every platform. It shows what would have happened without the platforms.
- **Percentage point:** the difference between two percentages. Going from 1% to 2% is an increase of 1 percentage point, even though it's a doubling.
- **95% confidence interval (CI):** the range the true value very likely falls in. If it includes zero, we can't rule out "no change".
- **Placebo test:** running the same analysis on fake platform locations to see how often a result this large happens by chance.

## Sources

- Slick data: [SkyTruth Cerulean](https://cerulean.skytruth.org), via its [public API](https://api.cerulean.skytruth.org) (no account needed)
- Platform locations: [BOEM Santa Ynez Unit Development and Production Plan](https://www.boem.gov/renewable-energy/9b5-1985-06-platforms-harmony-heritage-hondo-santa-ynez-unit-developmenet)
- Restart timeline: [World Oil, May 2025](https://www.worldoil.com/news/2025/5/19/sable-offshore-restarts-oil-production-from-santa-ynez-unit-10-years-after-shut-in/) · [Noozhawk](https://www.noozhawk.com/sable-restarts-oil-production-transport-in-santa-barbara-county-after-federal-order/) · [Oil & Gas Journal, April 2026](https://www.ogj.com/drilling-production/production-operations/news/55368298/sable-offshore-set-to-resume-oil-production-at-second-platform-offshore-california)
