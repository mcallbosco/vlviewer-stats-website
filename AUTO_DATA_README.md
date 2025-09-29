# Auto-generated Data Formats (auto/)

This repository contains auto-generated datasets under the `auto/` folder. Do not edit files in `auto/` by hand.

Contents:

- auto-index.json: Top-level index linking configurations to their artifact files
- auto/{slug}/voicelines-deltas.json: Time series of per-voiceline deltas
- auto/{slug}/conversations-deltas.json: Time series of per-conversation deltas
- auto/{slug}/character_totals-deltas.json: Time series of per-character total deltas
- auto/{slug}/id-to-text.json: Map of voiceline ID (filename) to transcript text

Delta files structure:

{
  "meta": { "config": "Games/.../config.json", "generated_at": "ISO", "downsampled_before": "ISO", "source_repo": "owner/repo" },
  "series": {
    "<id>": [ { "t": "ISO-timestamp", "d": <deltaNumber>, "c": <currentCountOptional> }, ... ]
  }
}

Downsampling policy:
- Recent window: last 5 days → keep 30-minute granularity
- Older than 5 days → aggregate into 3-hour buckets (sum deltas, keep last current count when present)

ID → Text mapping:
- Keys are audio filenames found in the website JSON (voicelines and conversations)
- Values are associated transcript strings, taking voicelines data when available, otherwise conversations
