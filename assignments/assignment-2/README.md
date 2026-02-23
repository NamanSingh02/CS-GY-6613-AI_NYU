# Assignment 2 — RAV4 Exterior Component Retrieval

**Student:** Naman Singh  
**Course:** AI Spring 2026  
**Submission date:** 22nd Feb 2026

## What this contains
- `assignment_2.ipynb` — notebook that:
  - Downloads only the 18:43–24:43 interval from the source YouTube video (timestamps in original video time)
  - Extracts frames every 5 seconds
  - Fine-tunes a part-level YOLO segmentation model using Ultralytics `carparts-seg`
  - Produces three Parquet files and uploads them to Hugging Face
- Hugging Face dataset repo (Parquets + README): 

**Hugging face commit ID**: 564b8c362cbe251e6854a981513908357727f758

https://huggingface.co/datasets/Naman02/rav4-exterior-retrieval-interval2

## Outputs (Hugging Face)
- `detections_by_frame.parquet` — one row per frame, `detections` list column
- `clips_by_query.parquet` — two columns: `query_image`, `youtube_url` (embed URL with start/end)
- `best_clips_by_query.parquet` — two columns: `query_image`, `youtube_url` (embed URL with start/end)

## How to run (Google Colab)
1. Open `assignment_2.ipynb` in Colab.
2. Set runtime to **GPU**.
3. Run cells top-to-bottom.
4. Authenticate:
   - Google Drive mount (for saving outputs)
   - Hugging Face token (for uploading Parquets)

## Notes
- Analysis is restricted to the interval 18:43–24:43 of the original video
- Frames sampled at 1 per 5 seconds.