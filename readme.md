# FSR 3 Frame-Generation Optimization Plan

**Target hardware:** GTX 1660 Ti  
**Target workload:** Non-ray-traced games  
**Primary goal:** Increase displayed FPS while maintaining or improving frame-generation quality.

## Primary Strategy

Keep FSR 3’s existing frame-generation pipeline, but make optical flow, validation, repair, and refinement adaptive instead of processing every pixel equally.

---

## 1. Establish the Baseline

### Checklist

- [X] Fork the FSR 3 repository and create a separate experimental branch.
- [X] Build the unmodified sample and integration.
- [X] Confirm frame generation works on the GTX 1660 Ti.
- [X] Record performance without frame generation.
- [X] Record performance with unmodified FSR 3 frame generation.
- [ ] Capture repeatable scenes:
  - [ ] Static scene
  - [ ] Slow camera movement
  - [ ] Fast camera pan
  - [ ] Character movement
  - [ ] Foliage
  - [ ] Particles
  - [ ] Transparency
  - [ ] Reflections
  - [ ] Camera cuts
  - [ ] UI-heavy scenes
- [ ] Capture GPU timestamps for every frame-generation pass.
- [ ] Record GPU memory usage.
- [ ] Record frame pacing and latency separately from displayed FPS.

### Baseline Spreadsheet

| Measurement | Current FSR 3 |
|---|---:|
| Real rendered FPS | Avg 70-80 |
| Displayed FPS with frame generation | Avg 120-130 |
| Frame-generation GPU time | Avg 14 ms  |
| Optical-flow GPU time |  |
| Interpolation GPU time | |
| Presentation/composition GPU time | |
| GPU memory used | |
| 99th-percentile frame time | |
| Input-to-display latency | |
| Ghosting score | |
| Disocclusion score | |
| Particle artifact score | |

> [!IMPORTANT]
> Do not modify the algorithm until the baseline is repeatable.

---

## 2. Add Instrumentation Before Changing Output

### Debug Views

- [ ] Motion-vector visualization
- [ ] Optical-flow visualization
- [ ] Depth visualization
- [ ] Forward/backward flow error
- [ ] Confidence map
- [ ] Disocclusion mask
- [ ] Difficulty/tile mask
- [ ] Source-frame selection mask
- [ ] Repair/refinement mask
- [ ] History-validity mask

### GPU Timing Markers

Add timestamps around:

```text
OpticalFlow
FlowDownsample
FlowRefinement
MotionVectorDilate
FrameReprojection
ConfidenceCalculation
DepthValidation
DisocclusionRepair
FrameBlend
Refinement
Sharpening
UIComposition

