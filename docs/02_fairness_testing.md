# 2. Fairness testing

## 2.1 Design

A separate black-box tester. It calls only the assessment interface the app itself uses (guard, AI, scoring rules,
cross-check), never reads the app's code, and the app does not know it exists.

- **Inputs:** fictional CVs, scored against a fictional company. No real candidate and no real demographic data is
  used, and nothing real is sent to the AI provider.
- **Variants:** each CV is re-scored with swapped names (seven origins, female and male), gendered pronouns, and
  added protected attributes (age, nationality and work permit, family, disability, religion, photo).
- **Noise first:** each CV is scored four times unchanged. The spread of those scores is the system's own noise.
- **Possible effect:** a group's mean difference from baseline is larger than both twice the noise and 3 points.
- **Leaks:** the AI's output mentions something the guard should have removed (a name, a nationality, a birth
  year). A leak means a redaction rule needs fixing.
- **Controls:** where a CV has a feature under test (a career gap, non-native English), an otherwise identical
  control CV without it.
- **Every report is kept**, as evidence for the data-protection impact assessment and the EU AI Act's data
  governance duty. The test is re-run after any change to the prompt, model, scoring rules or guard.

## 2.2 What the runs found

![Language effect](../assets/language_effect.svg)

| Run (2026) | n | Finding | What changed |
|---|---|---|---|
| 12 Oct | first screening | No effect beyond noise, but the candidate's name still reached the AI through a profile link, and one CV hit the score ceiling, which hides effects | Guard removes names from links; test CV adjusted |
| 13 Oct, attacks | 24 attacks | 0 of 24 raised the tier; fake credentials never reached the AI | None needed |
| 13 Oct, full | 408 | No group beyond noise. But two CVs scored bimodally (for example 55 vs 95) because one borderline yes/no signal flipped a rule cliff | Removed a score floor, counted each signal once, temperature 0 |
| 13 Oct, full | 408 | Non-native-English CV now stable (spread 18.9 → 2.3). Career-gap CV still split | Tightened one signal's definition |
| 13 Oct, full + control | 459 | No group beyond noise; gender gap +1.1 points over 126 pairs; impact ratios 0.88 to 1.00. Career gap: no effect (−0.3, 95% CI −0.5 to 0.0). Two CVs still unstable on one signal | Added a fluent-English control |
| 13 Oct, language | 102 | **Language effect: −5.2 points (95% CI −7.1 to −3.4)** for the non-native version of the same CV | Tightened that signal; majority of three answers |
| 13 Oct, full | 510 | Language effect down to **−1.8 points (95% CI −2.6 to −1.0)**, now through lower ratings on two other dimensions. A strong CV lost credit and two CVs became unstable | Split one signal into two smaller steps |
| 14 Oct, full | 74 of 510 (cut short) | Where data exists, scores stable; too little for group results | Full re-run still to do |

## 2.3 What this does and does not show

- **Shows:** the test finds real problems (a name leak, rule cliffs, a language effect) and shows whether a fix
  worked. That is its purpose.
- **Does not show:** that the system is free of bias. It is a screening test with few repeats per variant, on a
  small set of fictional CVs, against one fictional company.
- **Open:** the language effect is smaller but still present. The current version has not had a complete full run.
