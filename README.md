# Coach Cleaning Photo Record

Before/after cleaning photo capture for coach interiors, for the rake SSE/JE.
Runs in the phone browser. **All photos and data stay on the phone** — there is no
server, no account, and nothing is uploaded anywhere.

Built for Train Care Centre, Basin Bridge, Chennai-3.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app |
| `manifest.webmanifest` | Lets it install to the home screen |
| `sw.js` | Offline cache, so it opens in the yard with no signal |
| `icon.svg` | Home screen icon |

All four must sit in the same folder.

## Publishing

GitHub Pages, no git or command line needed:

1. New repository, **Public**.
2. **Add file → Upload files** → drag all four files in → Commit.
3. **Settings → Pages** → Source: *Deploy from a branch* → `main` / `(root)` → Save.

Live at `https://<username>.github.io/<repo>/`.

To update later: upload the new `index.html` the same way, and bump `CACHE`
in `sw.js` (`coachphoto-v1` → `v2`) so phones pick up the new version.

## How the SSE uses it

**Once per rake:** train number, date, name. Never re-entered.

**Before round.** Type the 6-digit coach number (stays until changed). Pick
Area → which one → item. Shoot. The coach number, area, item, phase and capture
time are burnt into the bottom of every photo.

**Between rounds.** Export → *Open worklist* → a page listing every before photo
grouped by coach with a blank "action taken" column. Share → Print → Save as PDF
to send the contractor.

**After round.** Flip to AFTER. The screen becomes the list of before photos.
Tap a pending row, the before photo fills the screen as a framing reference,
shoot, the row turns green. Pairing is created at the moment of shooting — it
cannot be mis-paired later.

**Export.** Share ZIP (or Download ZIP) → hand to CMIS.

> **Back up at the end of the before round, before leaving the rake.**
> The photos exist only on that phone until exported.

## What CMIS receives

A ZIP containing every photo named
`<train>_<coach>_<AREA>-<n>_<ITEM>_<BEFORE|AFTER>.jpg`,
plus `manifest.csv`:

```
file,train_no,coach_no,area,instance,item,phase,captured_at,pair_file,supervisor
```

`pair_file` on each AFTER row names its matching BEFORE file — that is what the
deck generator uses to build each before/after slide. File timestamps inside the
ZIP are the real capture times.

## Notes

- Photos are downscaled to 1600 px on the long edge (~400 KB each), which is
  ample for the slide boxes and keeps a full rake around 50 MB.
- Capture time is read from the camera's own EXIF at the moment of capture and
  stored in the manifest, because re-encoding the image drops EXIF.
- The area/item list is editable in the app under **Areas**.
