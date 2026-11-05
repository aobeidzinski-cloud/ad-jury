You are describing an ad image for a scoring tool. Attached: one ad image. Caption first line (may be empty): "[CAPTION]".

Write JSON with exactly these fields and nothing else:
- "id": "[FILE]"
- "hook_on_image": the exact words written on the image, keeping case and punctuation; "(none)" if there are no words
- "caption_first_line": the caption given above, or "(no caption provided)"
- "scene": at most 40 words: the setting, who is in it, what they are doing
- "story_object": a short noun phrase for the object carrying the story (phone, receipt, whiteboard, monitor, invoice pile); "(none)" if nothing carries it
- "format": "static", "video", or "carousel"

Describe only what is visible. Do not guess results. Do not judge quality.
