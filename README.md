# Breaze

A browser-only breathing practice timer built with React, TypeScript and Vite. Published at https://terapevti4no.github.io/breaze/.

## Development

```sh
npm ci
npm run dev
```

Local preview: http://127.0.0.1:4176/. Tests: `npm test`. Build: `npm run build -- --base=/breaze/`. Node 22.18+; CI uses Node 24.

## Practices

Click a preset card to start using the selected number of cycles and pre-start countdown. The default is four cycles with a five-second countdown; 0 begins immediately. Each cycle is inhale → hold after inhale → exhale → hold after exhale. Zero-second holds are skipped completely.

| Preset | Inhale | Hold | Exhale | Hold |
| --- | --- | --- | --- | --- |
| Balance / Равновесие | 5 | 0 | 5 | 0 |
| Gentle exhale / Плавный выдох | 4 | 0 | 6 | 0 |
| Box / Квадрат | 4 | 4 | 4 | 4 |
| Quiet pause / Тихая пауза (4–7–8) | 4 | 7 | 8 | 0 |

Your rhythm / Свой ритм lets you configure all four phases. Inhale and exhale accept 1–60 seconds; holds 0–60. Cycles: 1–100; countdown: 0–120. Custom settings and session options are remembered locally. Breath-phase numbers show the seconds remaining. The visual layer is intentionally numeric for this first version; `frameAt` already provides phase progress for a future animated circle.

Names and exact equal-breath timing are product presets, not claims of guaranteed clinical effects. Background reading: [NHS gentle equal-count breathing](https://www.nhs.uk/mental-health/self-help/guides-tools-and-activities/breathing-exercises-for-stress/), [NHS 4/6 breathing](https://www.cht.nhs.uk/services/clinical-services/oncology/information-and-support/prehabilitation/breathlessness-and-relaxation), [VA box breathing](https://www.veteranshealthlibrary.va.gov/LivingWith/BackNeck/Caring/3,84803), [NHS 4–7–8](https://www.nhs.uk/every-mind-matters/mental-wellbeing-tips/how-to-fall-asleep-faster-and-sleep-better/how-can-meditation-help-with-sleep/).

## Controls and timing

- Pause freezes the phase and exact remaining time; Resume continues from that point.
- Stop or Escape returns to the practice menu and restores keyboard focus to the last choice.
- Space pauses/resumes when a native interactive element is not focused; focused buttons retain their normal keyboard behavior.
- Changing tabs automatically pauses; returning never resumes automatically.
- Controls fade after inactivity, reappear on pointer movement/touch, and remain visible while keyboard-focused, paused or counting down.
- On supported mobile browsers, a screen wake lock is requested during active practice and released on pause/exit; denial does not interrupt practice.

`src/session.ts` is a pure phase engine and monotonic pause-aware clock. Time is based on `performance.now()`, not on counting timer callbacks. Tests cover exact phase boundaries, skipped holds, repeated cycles, final holds, pause/resume, countdown and malformed stored settings.

## Shared UI and preferences

The header, parent/tool wordmarks, text-only intro, content container, theme/language controls, information note and footer come from `@terapevti4no/ui`, installed from GitHub Releases. Breaze uses `breaze:v1`; missing language/theme fields fall back to `terapevti4no:v1`, then `emdrdot:v1`, then the browser. Its storage writes preserve the preferences record and never change the other tools' records. No accounts or server storage are used.

```sh
npm run ui:check
npm run ui:update -- --latest
```

The package is bundled into the static site; visits do not fetch UI code from GitHub. Do not copy library components or logo paths into this repository.

## Deployment

The GitHub Pages source is GitHub Actions. Pushes and PRs run tests and build; only `main` deploys. The workflow checks for new stable UI releases four times an hour, tests/builds updates, commits the verified dependency change and deploys in the same run. GitHub may delay scheduled jobs. Run **Build and deploy** manually for an immediate check. Set `UI_AUTO_UPDATE=false` to stop scheduled updates.
