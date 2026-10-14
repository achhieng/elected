# Elected ✓

A browser-based political campaign game inspired by *Prime Minister Infinity*. Choose a party leader, plan your campaign a week at a time across a real map of New Zealand, send surrogates out, spin the news, and watch the 1993 election count come in seat by seat.

Everything runs locally in the browser. No account, no server, no network needed once loaded.

## Quick start

```bash
npm install
npm run dev              # http://localhost:5173
npm test                 # 91 tests: electoral systems, campaign rules, data, import/export, election night
npm run build:single     # one self-contained HTML file in dist-single/index.html
npm run build:nz1993     # rebuild the New Zealand 1993 scenario from scripts/nz1993-source.ts
npm run balance          # play the scenario with idle and active strategies for each party
```

## The scenario: New Zealand, 6 November 1993

The last first-past-the-post election. 99 electorates (95 general, 4 Māori), 50 for a majority.

| Playable leader | Party | Seat | Goal |
|---|---|---|---|
| Rt Hon. Jim Bolger | National | King Country | 50 seats, 35% |
| Rt Hon. Mike Moore | Labour | Christchurch North | 50 seats, 35% |
| Jim Anderton MP | Alliance | Sydenham | 4 seats, 20% |
| Hon. Winston Peters | New Zealand First | Tauranga | 3 seats, 10% |

Only leaders who sit in Parliament can be played. Christian Heritage campaigns as a computer-run party; McGillicuddy Serious and the combined minor parties stand but don't campaign.

### What is real and what is estimated

* **Sourced** (Wikipedia, *1993 New Zealand general election*): national votes and seats for every party, turnout, every electorate's name, incumbent, winner, runner-up and majority, and the party leaders. The game's count of the stored votes reproduces the official result exactly: 50 / 45 / 2 / 2, and every electorate's winner and majority.
* **Inferred** (flagged in `scripts/nz1993-source.ts`): runner-up party where the source did not state it.
* **Last election (1990)** (Wikipedia, *1990 New Zealand general election*; `scripts/nz1990-source.ts`): national votes and seats, and each 1990 electorate's winner, runner-up and majority. Swings, holds and gains are measured against 1990, not against the real 1993 result. 1993 used new boundaries, so the 1990 result is *notional*: each seat's shares are estimated on the 1993 boundaries, keeping the real 1990 winner and majority wherever the electorate continued (new seats such as Howick are marked as new). The notional count gives National 69, Labour 29, Alliance 1 on the 99 seats (1990 actual: 67 / 29 / 1 of 97).
* **Candidate names**: winners, runners-up and incumbents are real. Full 1993 candidate lists are not published online, so the other candidates have invented names, marked as such in the game (hover the name).
* **Estimated**: each electorate's full vote breakdown (fitted to the sourced winner, runner-up and majority and to the national totals, which land within 0.5%), electors per electorate, demographics.
* **Map**: the real 1993 electorate boundaries, read from the SVG map “1993 New Zealand general election” by Erin (Wikimedia Commons, `scripts/data/`). Only the land shapes are kept — no legend, labels or insets. Scroll or pinch to zoom (up to 160×), drag to pan, double-click an electorate to zoom to it, or jump to Auckland, Hamilton, Wellington, Christchurch or Dunedin. The Māori electorates are a separate layer, scaled from the SVG’s Māori map onto the main map.
* **Game design**: leader and surrogate ratings, funds, campaign events and pollsters. Events are fictional.

Starting polls equal the final 1993 result, so an evenly fought campaign lands close to history. Party war chests are double their 1993 estimates (National $4.8m, Labour $2.8m, Alliance $0.9m, NZ First $0.7m) so there is money for a full advertising campaign.

## How the campaign works

Each turn is a **week** (six weeks from 25 September to polling day).

1. **Choose your policy focus**: up to three of the ten policy areas (Economy & Jobs, Healthcare, Education, Environment, Housing, Immigration, Crime & Justice, Defence, Transport, Welfare). It is free in week one; adding an issue later costs 8 CP. Your focus issues rise up the agenda when you rally, barnstorm or give interviews, earn voters' trust a little more each week, make 25% stronger ads, and make local campaigning 20% stronger in electorates where they are a local concern. Prominent issues build momentum for the parties voters trust on them, so pick ones you own. Rivals campaign on theirs.
2. **Plan the week.** Each campaigner has a **7-day calendar** (Saturday to Friday) with four action slots a day. Pick a day, then click electorates and add actions: travel, door knocking, rallies and so on. Nothing happens until you press *End week*; the week then plays out day by day, and **vote shares only move when the week ends** (published with the new polls), never while you plan. Planned actions reserve their **command points** at once (removing one refunds them). **Energy** (the ring around the portrait) is a weekly budget of 70 + 3 × Stamina (up to 100): it is spent day by day, does not come back overnight, and resets in full at the start of every week. **Rest** uses an action slot to get 10 + 1.5 × Stamina back. The calendar shows each day's energy before and after, and each new week opens on its first day. Command points: 40 + 6 × Leadership, refilled each week.

   | Action | Energy | CP | Money | What it does |
   |---|---|---|---|---|
   | Travel | 1–9 | — | by distance | Click an electorate, then *Travel*. Up to 250 km drives (1–5 energy); longer trips and Cook Strait crossings fly (5–9). The flag shows where the campaigner will be on the selected day. |
   | Door knock | 5 | 1 | $500 | Strongest local effect; Integrity and Stamina; diminishing returns |
   | Rally | 8 | 3 | $12k | The electorate, a little of the region, enthusiasm and your focus; hecklers are a risk |
   | Barnstorm | 14 | 4 | $6k | Every electorate in the region (two slots); Charisma |
   | Media interview | 4 | 3 | — | National: pushes your focus issues up the agenda; Charisma and Issue familiarity |
   | Fundraise | 6 | 2 | — | Up to $25,000, banked as soon as you plan it (cancelling takes it back); more in affluent electorates; Experience and Leadership. Recruited surrogates also raise money in the background every week without using slots or energy. |
   | Debate prep | 4 | 2 | — | Leader only, up to four a week; banks sessions for the next debate |
   | Rest | gives back 10 + 1.5 × Stamina | — | — | Uses a slot; energy back for later in the week |

   The Team tab switches which campaigner you are planning for. Surrogates left on *auto* fill their empty days themselves when the week ends, heading for your target seats, at no CP cost.
3. **Work the seats.** Hover an electorate for its opinion polling (predicted hold or gain, each party's share and change since the 1990 election). Click it to open the compact electorate panel: polling and the 1990 result side by side, quick buttons to plan a door knock, rally or trip there on the selected day, and the ground campaign in one row (the candidate list and tracking polls sit under *More*):
   * **Target seat** (4 CP) — your surrogates head there first, and the seat gets a steady push every week. Targets are outlined in gold on the map.
   * **Organisation** (5 CP per level, up to 5) — permanent: more votes, and every activity in the seat lands harder.
   * **Field campaign** (6 CP to launch, $8,000 a week) — volunteers on the streets every week; switch it on and off as money allows.
   * **Tracking poll** (3 CP + $3,000) — a private 500-voter poll of the seat, tracked week by week.
4. **Spend command points** on the wider campaign:
   * **Surrogates** (15 CP) — senior party figures (e.g. Helen Clark, Ruth Richardson, Sandra Lee, Tau Henare) who campaign for you (on auto, or directed from the Team tab).
   * **Ads** — **promote** your policies on an issue, or **attack** a rival party on an issue. Run them nationwide (10 CP, $60k a week), in a region (6 CP, $20k a week) or in one electorate (3 CP, $6k a week); they stay on air until you switch them off and make their issue more prominent. See *Voter blocs* below for how they move votes.
   * **Opposition research** (10 CP; finds can surface as stories later).
5. **End the week.** Rival parties act by the same rules, and any scheduled leaders' debate is held.
6. **Spin the news.** The week's stories appear before they're published. Spend 6 CP to soften a bad story about you, amplify a good one, or undercut a rival; success depends on your leader's Charisma and Integrity. Rivals spin too. Then publish, see the new polls and start the next week.

After six weeks comes **election night**, played like a live broadcast in real time. The polls close at 7pm with an exit poll: a real sample of the vote with sampling error *and* a systematic bias, so it can miss by several seats (the final card and graphics show how far off it was). Then the clock runs and every electorate counts at once: votes arrive in batches, a seat that is counting is shaded lightly in the leading party's colour, and when its lead can no longer be overturned (or the count is complete) it is called and filled solid. The card shows the votes counted so far and the percentage counted; a *Just called* list shows the latest calls, with HOLD or GAIN against 1990. Colour the map by **winner**, **margin** (pale for marginal seats, dark for safe ones, like printed election maps) or **gains**. Run the clock at 1×–16× (the whole count takes four minutes at 1×), pause, jump to the next call or to your leader's seat, or skip to the end. When the last seat is declared the result is recorded and a results card appears, but you stay on election night: choose *Stay on election night* to keep exploring the map, or *Continue to outcome*. You can come back to the night map from the outcome screen, or replay the count. **Image** saves the map as a PNG in the style of printed election maps: a block per party with its leader and leader's seat, the national vote % and seats, a bar of its share of the seats, a winning-margin colour ramp for each party (0–5, 5–10, 10–20, 20–30, 30–40, 40–50, 50%+) when coloured by margin, the Māori electorates and city insets; the outcome screen's results map does the same as PNG or SVG. **Excel** (on election night once the count is done, and on the outcome screen) saves the full result as a workbook: a *National* sheet (party, short name, colour, leader, leader's electorate and whether they held it, votes, vote %, change in vote %, seats and change in seats against 1990, with totals), an *Electorates* sheet (every seat: winner, winning party, majority, margin, turnout, votes cast, 1990 winner, hold or gain, and every party's votes and %), and a *Candidates* sheet. The outcome screen judges your goal, shows possible governments, and lets you continue into an alternate-history 1996 election under the same rules. Proportional and mixed systems (in mods) keep the batch-by-batch count view.

Candidates differ in local strength (their local popularity, not shown as stars): strong candidates win more votes in their seat. In 1993 strength comes from the real result: winners start highest, with more for a big majority or for being the sitting member, then runners-up; leaders are strongest. Sitting MPs are marked with a small **(Inc)** after their name.

The screen has a top bar (the game's chips and icon navigation) and, in a game, a status bar (poll average or result, the date and quick links), so the map gets the full width. Type is Inter with condensed figures. Campaign HQ is built around the map, and its highlights and glows take a shade of your party's colour. Map controls, polls, the House projection, issues and news float on the left. The player panel on the right has the portrait with its energy ring and command-point badge and four tabs: Player (the 7-day plan, actions for the selected day with their costs, skills, policy focus), Calendar (the six weeks, debates, last week's log), Party (money, spending, targets) and Team (campaigners). The electorate panel pops out when you click a seat, and the dock along the bottom holds Surrogates, Ads, Research, Polls and End week.

### Voter blocs: who can be persuaded

At the start each party's vote (the real 1993 result) is split into **solid** supporters (half of it), **leaners** (a quarter) and voters who are **undecided** but breaking that way for now (a quarter). So 25% of the electorate is undecided, and up to another 25% (the leaners) can be pulled back into play; solid voters don't move for ads. The *Voters* tab beside the polls shows each party's solid, leaning and undecided-breaking voters and how the leaners moved this week.

* **Promote ads** win undecided voters and turn them into your leaners. They work best on issues voters trust you on, and when the undecided pool is large.
* **Attack ads** knock a rival's leaners loose into the undecided pool. From there they break to whoever is doing best with undecided voters (some go back to the party attacked), so attacks pay most alongside a promotional campaign. They land hardest where the rival is weak and you are trusted. National attacks make the news, and an attack can **backfire** (more likely with a low-Integrity leader, or attacking a party on an issue voters trust it on).
* Each week leaners firm up a little into solid support, undecided voters start to settle as polling day nears, and a party whose vote falls loses leaners first.

**The campaign AI** (the strategist) plans every rival party's advertising each week with the same costs and rules. It promotes on its best issue when there are undecided voters to win, and attacks the rival it fights most for seats or for the same voters, on that rival's weakest issue. It **retaliates** against parties that attacked it last week, switches to promotion to shore up support when it loses leaners, and goes more negative when its leader's Integrity is low or it trails a close rival. It paces spending over the weeks left: rich parties run national ads, poorer ones run regional ads in the regions with their most contested seats. Because everything runs through the same blocs, campaigns can produce alternate-history results well away from 1993.

### Leader ratings (1–10)

| Rating | Affects |
|---|---|
| Leadership | Command points, fundraising |
| Integrity | Door knocking, spin, resistance to scandal and opposition research, heckling risk |
| Experience | Fundraising |
| Issue familiarity | Debate preparation and performance |
| Charisma | Rallies, barnstorming, ads, spin |
| Stamina | Weekly energy and how much a rest gives back |
| Debating | Debate performance |

Ratings are game ratings, not assessments of real people.

## Other modes

* **Election sandbox / alternate history** — set national vote shares, leader approval, the economy and uncertainty, or switch to any of seven electoral systems; simulate instantly or run 500 simulations.
* **Watch the real count** — election night for the actual 1993 result.
* **Scenario editor and mod library** — build your own election (the editor's *New nation* is a small playable template), with JSON/ZIP/CSV import and export, validation and migration. Leader ratings, titles, playability, goals and surrogates are editable.

## Data format

Scenario packages use the same layout as `/content`; see `docs/DATA_FORMAT.md` and the JSON Schemas in `docs/schema/`. New fields for this version: politician `title`, `ratings`, `tagline`; party `playable`, `goal`, `surrogateIds`, `startSeat`; constituency `geo` and `kind`; candidate `fictional`; scenario `lastElectionId` (the last election, optionally `notional` with `seatNotes`); issue `owners`; schedule `day`; campaign `hoursPerDay`; map type `svg` with `focus`, `layers`, `note` and `credit`. A package can also ship its map as an `.svg` file.

## Project layout

```
src/engine/   types, rng, electoral/*, election, voter, week (campaign), ground (seats, ads, issues), game, polling, events,
              forecast, night, government, previous
src/mods/     schema + validator, migration, package assembly, ZIP, CSV, template, file import
src/ui/       screens (Campaign HQ, map, polling, election night, results, editor…), components
content/      New Zealand 1993 scenario (generated)
scripts/      nz1993-source.ts (sourced data), build-nz1993.ts, svg-map.ts + data/ (the 1993 map SVG),
              nz1993-events.json, balance, schema export
tests/        Vitest suites
```
