# Well Log Analysis — Shale Volume and Log Quality

Well log data from a 192 m interval, sampled every 0.1 m. The project picks a
sand/shale cutoff from the data itself, calculates shale volume, and plots all
five curves including the density quality curve.

## Results

- **Sand: 104.3 m of 192.1 m (54%)**
- Average shale volume: 0.46
- Clean value 26 API, shale value 117 API, cutoff 70 API

## 1. Choosing the cutoff from the data

Gamma ray measures natural radioactivity. Clay minerals contain potassium and
absorb thorium and uranium, so shale reads high; clean quartz sand reads low.

![Gamma ray histogram](figures/gr_histogram.png)

The histogram shows two groups — a sand peak near 30 API and a broader shale
population from 80 to 120 API, with a dip between them at about 70 API. The
cutoff comes from that dip rather than from a textbook.

Clean and shale values are the 5th and 95th percentiles, so one odd reading
cannot stretch the scale.

## 2. Shale volume

Linear shale volume rescales gamma ray onto 0 to 1:

```
Vsh = (GR - GR_clean) / (GR_shale - GR_clean)
```

Subtract the clean value, divide by the range. This reads slightly high on
shale, because gamma ray response is not perfectly linear with clay content.
Empirical corrections exist, but their coefficients are fitted to field data
rather than derived, so the straightforward version is used here.

## 3. Log plot

![Log plot](figures/log_plot.png)

Depth increases downward, matching the well. The sonic and neutron scales are
reversed so porous intervals deflect the same direction on every track.

Track 5 is **DRHO**, the density correction. The density tool has a pad pressed
against the borehole wall; mudcake or a rough hole holds it off, and the tool
corrects for that gap using two detectors at different spacings. DRHO is the
size of that correction, so it describes the tool rather than the rock. Where it
runs past about 0.05 g/cc the pad was not making good contact and the density
reading is less reliable — mostly below 4,740 m in this well.

## 4. Sand thickness

Each sample covers 0.1 m of hole, so counting samples below the cutoff and
multiplying by 0.1 gives thickness.

## Notes

- The cutoff and endpoints come from this well's own histogram. On another well they would be read again.
- Gamma ray is an indicator, not proof. Some clean sands contain feldspar or mica and read hot without being shale.
- There is no resistivity curve here, so nothing in this analysis says anything about oil or gas — that would need water saturation.

## Files

```
Well_Log_Analysis.ipynb   the analysis
WellData.csv              DEPTH, GR, DT, RHOB, DRHO, NPHI
figures/                  generated plots
```

Python · NumPy · pandas · Matplotlib
