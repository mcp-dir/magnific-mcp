# Ferramentas

Magnific expõe 61 ferramentas.

### 1. `magnific_account_balance`
**Input**: nenhum input

User plan + credits. Check before paid generations. Returns plan.{tier,productName,isUnlimitedMode}, credits.{available,totalPlan,spent,hasExtraCredits}. No history.

### 2. `magnific_creations_wait`
**Input**: `identifiers`, `timeoutSeconds` (opcional)

Long-poll 1..8 creations until terminal or `timeoutSeconds` (default 25, max 25).

### 3. `magnific_creations_get`
**Input**: `creationIdentifier`

Single creation complete data — metadata + URLs (`url` full-res, `previewUrl` ~1024px, `thumbnailUrl` ~400px).

### 4. `magnific_creations_search`
**Input**: `from` (opcional), `reference` (opcional), `query` (opcional), `page` (opcional), `folderReference` (opcional), `fileType` (opcional), `toolNames` (opcional), `excludeToolNames` (opcional)

Search user creations as raw TOON text.

### 5. `magnific_creations_list`
**Input**: `from` (opcional), `reference` (opcional), `query` (opcional), `fileType` (opcional), `toolNames` (opcional)

Filterable gallery widget (image/video/audio).

### 6. `magnific_creations_request_upload`
**Input**: `mimeType`, `count` (opcional)

Step 1 for local/user files: get presigned PUT URL(s).

### 7. `magnific_creations_finalize_upload`
**Input**: `path` (opcional), `uploads` (opcional)

Step 2 after PUT from `creations_request_upload`: convert temp path(s) to creations.

### 8. `magnific_creations_upload_image`
**Input**: `url`

Upload public image URL in one step.

### 9. `magnific_creations_upload_file`
**Input**: `file`

Import a file the user attached in the host (e.g.

### 10. `magnific_creations_move`
**Input**: `targetFolderReference`, `creationIdentifiers`

Move creations into `targetFolderReference`.

### 11. `magnific_creations_like`
**Input**: `creationIdentifier`, `folderReference`, `unlike` (opcional)

Toggle heart/favorite reaction on a user-owned creation.

### 12. `magnific_creations_comment`
**Input**: `creationIdentifier`, `folderReference`, `text`, `replyToCommentId` (opcional)

Add a comment (or reply) to a user-owned creation.

### 13. `magnific_folders_list`
**Input**: `parentReference` (opcional), `onlyProjects` (opcional), `page` (opcional)

List folders/projects as raw TOON text.

### 14. `magnific_folders_get`
**Input**: `reference`

Fetch folder metadata by `reference`.

### 15. `magnific_folders_create`
**Input**: `name`, `parentReference` (opcional), `type` (opcional)

Create folder/project. No `parentReference`: workspace root. `type` defaults: project at root, folder when nested.

### 16. `magnific_folders_rename`
**Input**: `reference`, `name`

Rename a folder by `reference`.

### 17. `magnific_folders_delete`
**Input**: `reference`

Move a folder and its files to trash.

### 18. `magnific_project_report`
**Input**: `reference`, `days` (opcional)

English markdown status report for a project/folder.

### 19. `magnific_spaces_list`
**Input**: `query` (opcional), `ownership` (opcional), `page` (opcional), `perPage` (opcional)

Lean spaces catalog as raw TOON text.

### 20. `magnific_spaces_create`
**Input**: `name`, `description` (opcional)

Create empty Space. Share `space.webUrl`; only call `spaces_show` when inline preview is requested.

### 21. `magnific_spaces_state`
**Input**: `spaceId`, `selectedElementIds` (opcional), `selectedConnectionIds` (opcional), `currentPageId` (opcional), `pageId` (opcional), `scope` (opcional)

Read-only board context, raw TOON.

### 22. `magnific_spaces_get_nodes`
**Input**: `spaceId`, `nodeIds`, `currentPageId` (opcional)

Read-only. Like `spaces_state` but scoped to `nodeIds` + their connections. Cheaper than full board. Needs view perm.

### 23. `magnific_spaces_run`
**Input**: `spaceId`, `startNodeId`, `mode` (opcional)

Run Space workflow from `startNodeId`.

### 24. `magnific_spaces_run_status`
**Input**: `workflowRunIdentifier`, `timeoutSeconds` (opcional)

Poll `spaces_run`. `timeoutSeconds` long-polls up to 25s. In-progress returns `poll_after_seconds`; terminal call `creations_show`.

### 25. `magnific_spaces_edit`
**Input**: `query`, `spaceId`, `threadId` (opcional), `selectedElementIds` (opcional), `selectedConnectionIds` (opcional), `anchorElementId` (opcional), `anchorDirection` (opcional)

External MCP only. Start headless Space edit. Poll `spaces_edit_status` until allTerminal, then verify with `spaces_state`.

### 26. `magnific_spaces_edit_status`
**Input**: `operationId` (opcional), `threadId` (opcional), `runId` (opcional), `timeoutSeconds` (opcional)

Wait for `spaces_edit` like `creations_wait`: long-polls up to `timeoutSeconds` (default 25, max 25).

### 27. `magnific_images_generate`
**Input**: `prompt`, `mode` (opcional), `aspectRatio` (opcional), `references` (opcional), `count` (opcional), `resolution` (opcional), `quality` (opcional), `folderReference` (opcional)

Generate/edit images. No refs=TTI. references[] (max 12; see field docs). Defaults: mode=auto, ratio=1:1, count<=8. resolution/quality per images_models_list. UI clients: after generating, MUST call `creations_show` w…

### 28. `magnific_images_remove_background`
**Input**: `creationIdentifier`, `folderReference` (opcional)

Cut-out subject on transparent PNG.

### 29. `magnific_images_upscale`
**Input**: `creationIdentifier`, `scale` (opcional), `folderReference` (opcional)

AI upscale 2x/4x (Magnific, premium).

### 30. `magnific_images_skin_enhancer`
**Input**: `creationIdentifier`, `version` (opcional), `sharpen` (opcional), `smartGrain` (opcional), `skinDetail` (opcional), `optimizedFor` (opcional), `folderReference` (opcional)

AI skin enhancer. faithful preserves identity; creative reinterprets; flexible uses presets. Defaults: faithful, sharpen=0, smartGrain=0.

### 31. `magnific_images_relight`
**Input**: `creationIdentifier`, `lights`, `resolution` (opcional), `numImages` (opcional), `folderReference` (opcional)

AI relight. 1-4 lights with azimuth/elevation enums, type neutral|gel, intensity 1-10. Gel uses hex `color`.

### 32. `magnific_images_change_camera`
**Input**: `creationIdentifier`, `rotate` (opcional), `vertical` (opcional), `closeup` (opcional), `folderReference` (opcional)

Reframe camera around subject.

### 33. `magnific_images_variations`
**Input**: `creationIdentifier`, `variationMode` (opcional), `prompt` (opcional), `gridRows` (opcional), `gridCols` (opcional), `aspectRatio` (opcional), `resolution` (opcional), `selectedAngles` (opcional), `selectedEthnicities` (opcional), `selectedGenders` (opcional), `folderReference` (opcional)

Generate image-variation grid.

### 34. `magnific_images_crop`
**Input**: `creationIdentifier`, `aspectRatio`, `folderReference` (opcional)

Center-crop to `aspectRatio`. Pixels: `images_resize`. AI upscale: `images_upscale`. Ratios: 1:1, 16:9, 9:16, 4:3, 3:4, 3:2, 2:3, 21:9.

### 35. `magnific_images_resize`
**Input**: `creationIdentifier`, `width`, `height`, `folderReference` (opcional)

Resize to exact pixels (no AI).

### 36. `magnific_images_to_svg`
**Input**: `creationIdentifier`, `folderReference` (opcional)

Trace raster creation into SVG.

### 37. `magnific_images_generate_svg`
**Input**: `prompt`, `aspectRatio` (opcional), `folderReference` (opcional)

Text-to-SVG (Recraft v4 Pro Vector).

### 38. `magnific_images_models_list`
**Input**: `search` (opcional), `onlyRecommended` (opcional)

TTI model catalog as lean TOON text.

### 39. `magnific_library_list`
**Input**: `scope` (opcional), `type` (opcional), `page` (opcional), `perPage` (opcional), `search` (opcional)

Lean library catalog (characters, styles, elements, locations) for agent reasoning.

### 40. `magnific_library_create`
**Input**: `name`, `type`, `description` (opcional), `gender` (opcional), `productType` (opcional), `images`

Create a reusable library asset (character/product/location) from 1-6 images.

### 41. `magnific_models3d_generate`
**Input**: `creationIdentifier`, `model` (opcional), `resolution` (opcional), `textureQuality` (opcional), `faceLimit` (opcional), `folderReference` (opcional)

Image-to-3D GLB. Upload external images first. Animate with `models3d_rig`. Models: tripo-p1 default, tripo-v31, trellis-2.

### 42. `magnific_video_plan`
**Input**: `prompt`, `durationHint` (opcional), `styleHint` (opcional), `aspectRatioHint` (opcional), `referenceIdentifiers` (opcional)

Call FIRST for any video request.

### 43. `magnific_video_generate`
**Input**: `video`, `folderReference` (opcional)

Generate video. External clients: call `video_plan` first to draft the brief and resolve model choice — skip only if the user explicitly says "just generate" or "one-shot". Pick a model with `slug` (copy it verbatim f…

### 44. `magnific_video_models_list`
**Input**: `search` (opcional), `onlyRecommended` (opcional)

Video-gen model catalog as lean TOON text.

### 45. `magnific_video_upscale`
**Input**: `creationIdentifier` (opcional), `videoUrl` (opcional), `mode`, `upscaleFactor` (opcional), `targetFps` (opcional), `targetResolution` (opcional), `focus` (opcional), `sharpen` (opcional), `enhancementModel` (opcional), `frameInterpolation` (opcional), `creativity` (opcional), `magnificResolution` (opcional), `premiumQuality` (opcional), `fpsBoost` (opcional), `smartGrain` (opcional), `flavor` (opcional), `turbo` (opcional), `preview` (opcional), `strength` (opcional), `folderReference` (opcional)

Upscale video via Topaz or Magnific.

### 46. `magnific_video_upscale_models_list`
**Input**: nenhum input

Video upscale mode catalog as raw TOON text (topaz, magnific, magnific_precision) with required/optional params, ranges and enums.

### 47. `magnific_video_concatenate`
**Input**: `name` (opcional), `creationIdentifiers`, `folderReference` (opcional)

Concatenate completed video creations into one MP4.

### 48. `magnific_video_speak`
**Input**: `audioUrl`, `videoUrl` (opcional), `imageUrl` (opcional), `mode` (opcional), `resolution` (opcional), `prompt` (opcional), `voiceId` (opcional), `folderReference` (opcional)

Make a character speak. Image+audio → talking head (Veed Fabric 1.0). Video+audio → lip sync (Lipsync 2.0). Omit `mode` for auto. Pass asset URLs or a creation `identifier` (from `audio_tts`, `images_generate`, `video…

### 49. `magnific_audio_tts`
**Input**: `text`, `voiceId` (opcional), `speakers` (opcional), `model` (opcional), `stability` (opcional), `similarityBoost` (opcional), `speed` (opcional), `useSpeakerBoost` (opcional), `temperature` (opcional), `systemInstruction` (opcional), `visible` (opcional), `folderReference` (opcional)

TTS voiceover. Single (voiceId) or 2-speaker (speakers). ElevenLabs/Google. No voiceId? Call `audio_voices_show` first (or `audio_voices_list` for agent-only lookup).

### 50. `magnific_audio_music_generate`
**Input**: `prompt`, `model` (opcional), `durationSeconds` (opcional), `instrumental` (opcional), `folderReference` (opcional)

AI music generation. Google Lyria or ElevenLabs. Pass `prompt`, optional `model`/`durationSeconds`/`instrumental`. Lyria/Lyria-3 produce fixed 30s; Lyria-3-Pro accepts 30–180s; ElevenLabs accepts 10–300s.

### 51. `magnific_audio_voices_list`
**Input**: `search` (opcional)

TTS voice catalog as raw TOON text, no UI.

### 52. `magnific_design_auto_resize`
**Input**: `creationIdentifier`, `aspectRatios`, `name` (opcional), `textLayers` (opcional), `imageLayers` (opcional), `numLayers` (opcional), `multipage` (opcional)

Create resized design pages from an image.

### 53. `magnific_design_auto_layers`
**Input**: `creationIdentifier`, `name` (opcional), `numLayers` (opcional), `vectorize` (opcional)

Extract editable layers from a flat image into a Pikaso design.

### 54. `magnific_flows_list`
**Input**: `query` (opcional), `ownership` (opcional), `page` (opcional), `perPage` (opcional)

Flows catalog as raw TOON text, no UI.

### 55. `magnific_flows_get`
**Input**: `identifier`

Flow input/output spec as raw TOON text.

### 56. `magnific_flows_run`
**Input**: `identifier`, `inputs`

Run flow. `flows_get` first for input specs. `inputs` is `{inputId: value}` (creation inputs from `creations_list`, voices from `audio_voices_show`). Returns `workflowRunIdentifier` → `flows_wait` → `creations_show`.

### 57. `magnific_flows_wait`
**Input**: `workflowRunIdentifier`, `timeoutSeconds` (opcional)

Long-poll a `flows_run`. Terminal: one entry per output creation with real `identifier` for `creations_show`. In-progress: `poll_after_seconds`.

### 58. `magnific_stock_search`
**Input**: `query`, `content_type` (opcional), `page` (opcional), `per_page` (opcional)

Search Freepik stock catalog by keyword.

### 59. `magnific_stock_get`
**Input**: `id`, `type` (opcional), `ids` (opcional)

Full details for a single Freepik stock item: preview URL, dimensions, formats, author.

### 60. `magnific_stock_download`
**Input**: `id`, `type` (opcional), `folder_reference` (opcional), `ids` (opcional)

Signed download URL for a stock item.

### 61. `magnific_stock_to_creation`
**Input**: `id`, `folder_reference` (opcional), `title` (opcional), `ids` (opcional)

Convert Freepik stock item → Pikaso creation.

## Prompts de exemplo

```
Faça upscale dessa imagem no Magnific
Liste minhas criações recentes no Magnific
Gere uma variação dessa imagem no Magnific
```
