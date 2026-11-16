# Issue

**Title:** Bringing back the positivity boost & space nerd vibes

## Description

## Summary

The new web UI lost the two fun "vibes" from the old Streamlit UI. This brings them back:

- **Positivity boost**: hearts and pluses float up behind the page, and the "agent is working" dots become 🐾.
- **Space nerd**: rockets rise, UFOs fly across (a rocket that hits a UFO goes 💥), and the working dots become 📡.
- **Professional** (the default): looks exactly as it does today.

Pick a vibe from the new **Vibe** dropdown at the bottom of the sidebar. The same sliders as before set how many hearts, pluses, rockets and UFOs appear. Your choice is remembered in your browser.

The emojis sit behind everything and ignore clicks, so the layout and controls are unchanged. They are hidden when the system is set to reduce motion.

---

<details>
<summary>Technical details</summary>

- `webui/src/vibe.ts`: vibe type, defaults, localStorage load/save (`scilink.vibe`), spinner glyphs, `VibeContext`.
- `webui/src/components/VibeLayer.tsx`: generates the floating spans (same size/speed/opacity ranges as `scilink/ui/theme.py`) and runs the rocket–UFO collision check on a 150 ms interval, cleaned up on unmount or vibe change.
- `app.css`: `.vibe-layer` is `position: fixed; z-index: -1; pointer-events: none`, so it paints over the page background and under everything else, with no change to any existing box. Keyframes are ported from the Streamlit CSS; there's a `prefers-reduced-motion` rule.
- `Sidebar.tsx`: Vibe dropdown and sliders above Quit App. `ChatPanel.tsx`: the spinner glyph is read from `VibeContext`. `App.tsx`: holds the state and the provider.
- Frontend only. Nothing goes to the server and no backend files changed.

**Checked:** `tsc --noEmit` and `npm run build` pass. A headless Chromium run against a local `scilink-web` showed both vibes rendering behind the welcome screen, the choice persisting across reloads, no console errors, and 7 collisions in 40 s at 50 rockets / 10 UFOs. Also checked by hand in the browser.

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `207e6938b726`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.