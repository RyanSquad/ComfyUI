---
name: h3prompt
description: Write MiniMax H3 audiovisual prompts for T2VA, I2VA, FL2VA, L2VA, and Ref2VA. Use when the user invokes /h3prompt or requests H3 prompt rewriting, keyframe alignment, reference labels, soundscape, or non-diegetic music.
disable-model-invocation: true
compatibility: Portable prompt-writing guidance; it requires no external API, MiniMax Hub tool, or proprietary runtime.
---

# MiniMax H3 Prompt Writing

## Workflow

1. Identify the input mode: T2VA, I2VA, FL2VA, L2VA, or full-reference Ref2VA.
2. For T2VA, I2VA, FL2VA, and L2VA, read [base-en.txt](references/base-en.txt).
3. For Ref2VA, read [ref-en.txt](references/ref-en.txt).
4. Preserve the exact field names, section order, reference labels, and timing notation required by the selected guide.
5. Write the requested prompt in English. Preserve dialogue, lyrics, and visible scene text in its original language.

## Mode selection

- **T2VA**: Build the complete audiovisual timeline from text.
- **I2VA**: Start from the supplied first frame and develop forward.
- **FL2VA**: Connect supplied first and last frames with a continuous visual path.
- **L2VA**: Infer a plausible opening and converge on the supplied last frame.
- **Ref2VA**: Use full-reference sections and stable labels for subjects, pictures, videos, and audio.

## Base-mode output

Use these fields in this order:

```text
integrated_multimodal_description: [Shot 1] ...

overall_soundscape: ...

non_diegetic_music: ...
```

I2VA must begin with:

```text
For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.
```

FL2VA must begin with the first/last alignment sentence and L2VA must begin with the last-frame alignment sentence described in `references/base-en.txt`.

## Full-reference output

Use these six sections in this exact order:

```text
subject_definitions:

summary:

retention_analysis:

detailed_description:

overall_soundscape:

non_diegetic_music:
```

Keep labels stable across every section:

- `<Subject N>`: reusable visible content such as people, objects, scenes, clothing, styles, or actions.
- `<Picture N>`: a concrete frame, keyframe, storyboard, or composition anchor.
- `<Video N>`: a source video or its temporal/camera structure.
- `<Audio N>`: a standalone audio asset or referenced synchronized audio.
- `(S1)`, `(S2)`, etc.: stable speaker IDs assigned by the order of actual vocal events.

## Writing rules

- Describe each shot by composition, subjects, environment, lighting, actions, camera, sound, and reference effects.
- Do not write a plot summary instead of a timeline.
- Do not introduce unresolved reference labels.
- Do not add timestamps to `[Shot 1]`; later shots use strictly increasing cut times within the video duration.
- Express camera movement naturally with motion type, amplitude, and speed when meaningful.
- Put actual dialogue and lyrics inside `<d>[Language] ... </d>` and preserve the user's words and original language.
- Put visible on-screen text in English double quotation marks and preserve its original text verbatim.
- `overall_soundscape` contains 1–4 English sentences about ambience, physical action sounds, and non-verbal human sounds. Do not repeat dialogue there.
- `non_diegetic_music` contains 1–3 English sentences about audience-only music, including instrumentation, tempo, rhythm, and dynamics. Use `N/A` when absent.
- Use `N/A` for a category only when the user requests or clearly implies that the category is absent.
- For keyframes, preserve identity, clothing, colors, objects, spatial relationships, and scene anchors unless the user requests a change.

## Validation

Before returning a prompt, verify:

- The selected mode matches the supplied assets.
- Required sections and field names are present and ordered correctly.
- Every reference label has one stable meaning.
- Timing matches the requested duration and keyframe positions.
- Dialogue, lyrics, and visible text were not translated or invented.
- Sound is separated correctly between shot-specific description, `overall_soundscape`, and `non_diegetic_music`.
