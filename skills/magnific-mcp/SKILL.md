---
name: magnific-mcp
description: Skill da REST API do Magnific na MCP.AI: 61 endpoints em /api/magnific. Magnific por linguagem natural: faça upscale de imagens, gere e liste suas criações. Conecte com sua conta Magnific e use os créditos do seu plano. Autentique com workspace API key (sk_live) gerada em app.mcp.ai/settings/api-keys. Use quando o usuário pedir algo coberto pelos endpoints.
---

# Magnific — REST API skill

Você tem acesso à **Magnific** REST API na MCP.AI.

> Magnific por linguagem natural: faça upscale de imagens, gere e liste suas criações. Conecte com sua conta Magnific e use os créditos do seu plano.

## Base URL

```
https://api.mcp.ai/api/magnific
```

Todo endpoint é um **POST** na Base URL + o path abaixo. Os parâmetros vão no corpo JSON.

## Autenticação

Inclua em toda request:

```
Authorization: Bearer sk_live_...
Content-Type: application/json
```

> Gere sua chave em **https://app.mcp.ai/settings/api-keys** (workspace API key `sk_live_…`, não expira, revogável). Uma única chave serve pra todos os seus MCPs.

## Formato de resposta

```json
{ "ok": true, "tool": "<tool_id>", "result": <payload> }
```

## Exemplo cURL

```bash
curl -X POST https://api.mcp.ai/api/magnific/account/balance \
  -H "Authorization: Bearer sk_live_..." \
  -H "Content-Type: application/json" \
  -d '{}'
```

## Reportar problemas

Se um endpoint retornar erro, vazio ou dado inesperado, reporte (não desista calado): **POST /api/magnific/report** com `{ "message": "...", "context"?: "...", "conversation"?: [...] }`. Isso notifica o time da MCP.AI.

## Endpoints (61)

#### `magnific_account_balance`

User plan + credits. Check before paid generations. Returns plan.{tier,productName,isUnlimitedMode}, credits.{available,totalPlan,spent,hasExtraCredits}. No history. _(POST /api/magnific/account/balance)_

#### `magnific_audio_music_generate`

AI music generation. Google Lyria or ElevenLabs. Pass `prompt`, optional `model`/`durationSeconds`/`instrumental`. Lyria/Lyria-3 produce fixed 30s; Lyria-3-Pro accepts 30–180s; ElevenLabs accepts 10–3 _(POST /api/magnific/audio/music/generate)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `prompt` | string | Sim | Music description. Min 10 chars, max 2000. Describe genre, mood, instruments, tempo. |
| `model` | string | Não | Default: google-lyria. google-lyria/google-lyria-3 = fixed 30s. google-lyria-3-pro = 30–180s. elevenlabs-music-generation = 10–300s. (google-lyria, google-lyria-3, google-lyria-3-pro, elevenlabs-music-generation) |
| `durationSeconds` | integer | Não | Duration in seconds. Required when using elevenlabs-music-generation. Only effective for google-lyria-3-pro (30–180) and elevenlabs-music-generation (10–300). Ignored for fixed-duration models. |
| `instrumental` | boolean | Não | Omit vocals. Only applicable for google-lyria-3 and google-lyria-3-pro. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_audio_tts`

TTS voiceover. Single (voiceId) or 2-speaker (speakers). ElevenLabs/Google. No voiceId? Call `audio_voices_show` first (or `audio_voices_list` for agent-only lookup). _(POST /api/magnific/audio/tts)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `text` | string | Sim | Max 40k. Eleven v3 best <=3k; turbo <=10k. `(pause 1.5s)` only on turbo. |
| `voiceId` | integer | Não | From voices catalog; required without `speakers`. |
| `speakers` | object[] | Não | 2 speakers, same provider. |
| `model` | string | Não | Default: eleven_v3 or gemini_v2_5_pro. (eleven_turbo_v2_5, eleven_v3, gemini_v2_5_pro, gemini_v3_1_flash_tts) |
| `stability` | number | Não | ElevenLabs 0-1; default 0.5. |
| `similarityBoost` | number | Não | ElevenLabs 0-1; default 0.2. |
| `speed` | number | Não | 0.7-1.2; default 1.0. |
| `useSpeakerBoost` | boolean | Não | ElevenLabs; default true. |
| `temperature` | number | Não | Gemini 0-2; default 1.0. |
| `systemInstruction` | string | Não | Gemini; max 10k. |
| `visible` | boolean | Não |  |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_audio_voices_list`

TTS voice catalog as raw TOON text, no UI. _(POST /api/magnific/audio/voices/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `search` | string | Não | Optional case-insensitive filter on voice name, description, gender, or language. |

#### `magnific_creations_comment`

Add a comment (or reply) to a user-owned creation. _(POST /api/magnific/creations/comment)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Creation identifier the comment will be attached to. |
| `folderReference` | string | Sim | `folderReference` from `creations_search`/`creations_get`. |
| `text` | string | Sim | Comment body (plain text, 1–5000 chars). |
| `replyToCommentId` | string | Não | Existing comment id to reply to; omit for new thread. |

#### `magnific_creations_finalize_upload`

Step 2 after PUT from `creations_request_upload`: convert temp path(s) to creations. _(POST /api/magnific/creations/finalize/upload)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `path` | string | Não | Single path from `creations_request_upload`. |
| `uploads` | object[] | Não | Batch 1-100. |

#### `magnific_creations_get`

Single creation complete data — metadata + URLs (`url` full-res, `previewUrl` ~1024px, `thumbnailUrl` ~400px). _(POST /api/magnific/creations/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Creation identifier. |

#### `magnific_creations_like`

Toggle heart/favorite reaction on a user-owned creation. _(POST /api/magnific/creations/like)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Creation identifier (from `creations_search`/`creations_get`). |
| `folderReference` | string | Sim | `folderReference` from `creations_search`/`creations_get`. |
| `unlike` | boolean | Não | Set `true` to remove the existing heart. Default `false` (add a heart). |

#### `magnific_creations_list`

Filterable gallery widget (image/video/audio). _(POST /api/magnific/creations/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `from` | string | Não | Source section. Default history; folder/project-root need `reference`. (history, folder, project-root, all-assets, favorites, downloads, trash, upload, v3-upload, v3-favorites, v3-trash) |
| `reference` | string | Não | Folder or project reference (required when from=folder or from=project-root). |
| `query` | string | Não | Search text to pre-fill the search bar. |
| `fileType` | string | Não | Pre-select a media type filter. (image, video, audio) |
| `toolNames` | string[] | Não | Pre-filter to specific tool slugs. |

#### `magnific_creations_move`

Move creations into `targetFolderReference`. _(POST /api/magnific/creations/move)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `targetFolderReference` | string | Sim | Destination folder ref. User-owned. |
| `creationIdentifiers` | string[] | Sim | Creation identifiers (max 200). Foreign identifier → abort. |

#### `magnific_creations_request_upload`

Step 1 for local/user files: get presigned PUT URL(s). _(POST /api/magnific/creations/request/upload)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `mimeType` | string | Sim | File MIME type. (image/jpeg, image/png, image/webp, video/mp4, video/quicktime, video/webm, video/x-m4v) |
| `count` | integer | Não | Batch count; omitted returns one upload. |

#### `magnific_creations_search`

Search user creations as raw TOON text. _(POST /api/magnific/creations/search)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `from` | string | Não | Default history; folder/project-root need `reference`. (history, folder, project-root, all-assets, favorites, downloads, trash, upload, v3-upload, v3-favorites, v3-trash) |
| `reference` | string | Não | Folder/project reference. |
| `query` | string | Não | Name filter. |
| `page` | integer | Não |  |
| `folderReference` | string | Não | Folder/project filter. |
| `fileType` | string | Não | Media type. (image, video, audio) |
| `toolNames` | string[] | Não | Include tool slugs. |
| `excludeToolNames` | string[] | Não | Exclude tool slugs. |

#### `magnific_creations_upload_file`

Import a file the user attached in the host (e.g. _(POST /api/magnific/creations/upload/file)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `file` | object | Sim | Host file reference (JPEG/PNG/WebP, max 25 MB). |

#### `magnific_creations_upload_image`

Upload public image URL in one step. _(POST /api/magnific/creations/upload/image)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `url` | string | Sim | Public image URL. |

#### `magnific_creations_wait`

Long-poll 1..8 creations until terminal or `timeoutSeconds` (default 25, max 25). _(POST /api/magnific/creations/wait)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `identifiers` | string[] | Sim | 1..8 creation identifiers. |
| `timeoutSeconds` | integer | Não | Long-poll budget (default 25). |

#### `magnific_design_auto_layers`

Extract editable layers from a flat image into a Pikaso design. _(POST /api/magnific/design/auto/layers)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Identifier of the source image. |
| `name` | string | Não | Descriptive 2-6 word name from the image subject. Required. |
| `numLayers` | integer | Não | Max layers (2-8, default 6). |
| `vectorize` | boolean | Não | Vectorize image layers (default false). |

#### `magnific_design_auto_resize`

Create resized design pages from an image. _(POST /api/magnific/design/auto/resize)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Source image identifier. |
| `aspectRatios` | string[] | Sim | Unique target ratios. |
| `name` | string | Não | 2-6 word name. |
| `textLayers` | boolean | Não | Default true. |
| `imageLayers` | boolean | Não | Default true. |
| `numLayers` | integer | Não | 2-8. |
| `multipage` | boolean | Não | Default true; false fans out. |

#### `magnific_flows_get`

Flow input/output spec as raw TOON text. _(POST /api/magnific/flows/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `identifier` | string | Sim | From flows_list/flows_show. |

#### `magnific_flows_list`

Flows catalog as raw TOON text, no UI. _(POST /api/magnific/flows/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `query` | string | Não |  |
| `ownership` | string | Não | mine (default) = own + saved + shared. public = community. (mine, public) |
| `page` | integer | Não |  |
| `perPage` | integer | Não |  |

#### `magnific_flows_run`

Run flow. `flows_get` first for input specs. `inputs` is `{inputId: value}` (creation inputs from `creations_list`, voices from `audio_voices_show`). Returns `workflowRunIdentifier` → `flows_wait` → ` _(POST /api/magnific/flows/run)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `identifier` | string | Sim | From flows_list/flows_show. |
| `inputs` | object | Sim | {inputId: value} from `flows_get`. |

#### `magnific_flows_wait`

Long-poll a `flows_run`. Terminal: one entry per output creation with real `identifier` for `creations_show`. In-progress: `poll_after_seconds`. _(POST /api/magnific/flows/wait)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `workflowRunIdentifier` | string | Sim | Run identifier from `flows_run`. |
| `timeoutSeconds` | integer | Não | Long-poll budget (default 25). |

#### `magnific_folders_create`

Create folder/project. No `parentReference`: workspace root. `type` defaults: project at root, folder when nested. _(POST /api/magnific/folders/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Sim | Display name. |
| `parentReference` | string | Não | Parent ref. |
| `type` | string | Não | `project`|`folder`. Inferred from parentReference. |

#### `magnific_folders_delete`

Move a folder and its files to trash. _(POST /api/magnific/folders/delete)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `reference` | string | Sim | Folder reference. |

#### `magnific_folders_get`

Fetch folder metadata by `reference`. _(POST /api/magnific/folders/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `reference` | string | Sim | Folder reference. |

#### `magnific_folders_list`

List folders/projects as raw TOON text. _(POST /api/magnific/folders/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `parentReference` | string | Não | List children; exclusive with `onlyProjects`. |
| `onlyProjects` | boolean | Não | Top-level projects only. |
| `page` | integer | Não |  |

#### `magnific_folders_rename`

Rename a folder by `reference`. _(POST /api/magnific/folders/rename)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `reference` | string | Sim | Folder reference. |
| `name` | string | Sim | New display name. |

#### `magnific_images_change_camera`

Reframe camera around subject. _(POST /api/magnific/images/change/camera)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Image creation identifier (not a file/upload id — import those first) |
| `rotate` | integer | Não | 0-360; default 45. |
| `vertical` | integer | Não | -30..90; default 0. |
| `closeup` | integer | Não | 0..10; default 5. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_crop`

Center-crop to `aspectRatio`. Pixels: `images_resize`. AI upscale: `images_upscale`. Ratios: 1:1, 16:9, 9:16, 4:3, 3:4, 3:2, 2:3, 21:9. _(POST /api/magnific/images/crop)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Image creation identifier (import uploads first). |
| `aspectRatio` | string | Sim | Target ratio. (1:1, 16:9, 9:16, 4:3, 3:4, 3:2, 2:3, 21:9) |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_generate`

Generate/edit images. No refs=TTI. references[] (max 12; see field docs). Defaults: mode=auto, ratio=1:1, count<=8. resolution/quality per images_models_list. UI clients: after generating, MUST call ` _(POST /api/magnific/images/generate)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `prompt` | string | Sim | Prompt. |
| `mode` | string | Não | Model `slug` from images_models_list. Omit or use 'auto' to let the server pick the best model. |
| `aspectRatio` | string | Não | Default 1:1; edits may snap. (1:1, 21:9, 16:9, 9:16, 2:3, 3:4, 1:2, 2:1, 5:4, 4:5, 3:2, 4:3) |
| `references` | object[] | Não | Max 12. |
| `count` | integer | Não | 1..8. |
| `resolution` | string | Não | Optional. Supported values per model — see `images_models_list.resolutions`. Omitted values use the model default. |
| `quality` | string | Não | Optional. Supported values per model — see `images_models_list.qualities`. Omitted values use the model default. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_generate_svg`

Text-to-SVG (Recraft v4 Pro Vector). _(POST /api/magnific/images/generate/svg)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `prompt` | string | Sim | Vector image prompt. |
| `aspectRatio` | string | Não | Aspect ratio (default 1:1). (1:1, 2:1, 1:2, 3:2, 2:3, 4:3, 3:4, 5:4, 4:5, 16:9, 9:16) |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_models_list`

TTI model catalog as lean TOON text. _(POST /api/magnific/images/models/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `search` | string | Não | Case-insensitive filter on name, slug, description, or tags. |
| `onlyRecommended` | boolean | Não | When true, returns only models with an `agentRecommendation`. |

#### `magnific_images_relight`

AI relight. 1-4 lights with azimuth/elevation enums, type neutral|gel, intensity 1-10. Gel uses hex `color`. _(POST /api/magnific/images/relight)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Image creation identifier (import uploads first). |
| `lights` | object[] | Sim | 1-4 lights. |
| `resolution` | string | Não | 1k or 2k default. (1k, 2k) |
| `numImages` | integer | Não | Default 1. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_remove_background`

Cut-out subject on transparent PNG. _(POST /api/magnific/images/remove/background)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Image creation identifier (import uploads first). |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_resize`

Resize to exact pixels (no AI). _(POST /api/magnific/images/resize)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Image creation identifier (import uploads first). |
| `width` | integer | Sim | px; snaps to multiples of 8. |
| `height` | integer | Sim | px; snaps to multiples of 8. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_skin_enhancer`

AI skin enhancer. faithful preserves identity; creative reinterprets; flexible uses presets. Defaults: faithful, sharpen=0, smartGrain=0. _(POST /api/magnific/images/skin/enhancer)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Image creation identifier (import uploads first). |
| `version` | string | Não | Default 'faithful'. (faithful, creative, flexible) |
| `sharpen` | integer | Não | 0-100; default 0. |
| `smartGrain` | integer | Não | 0-100; default 0. |
| `skinDetail` | integer | Não | faithful only. |
| `optimizedFor` | string | Não | flexible only. (enhance_skin, enhance_everything, improve_lighting, transform_to_real, no_make_up) |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_to_svg`

Trace raster creation into SVG. _(POST /api/magnific/images/to/svg)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Raster image identifier. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_upscale`

AI upscale 2x/4x (Magnific, premium). _(POST /api/magnific/images/upscale)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Image creation identifier (import uploads first). |
| `scale` | string | Não | Scale factor (default 2). (2x, 4x) |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_images_variations`

Generate image-variation grid. _(POST /api/magnific/images/variations)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Source image identifier. |
| `variationMode` | string | Não | Default 'angles'. (angles, demographics, expressions, age, storyboard, custom) |
| `prompt` | string | Não | Required for custom; used for storyboard. |
| `gridRows` | integer | Não | Default 3. |
| `gridCols` | integer | Não | Default 3. rows*cols <= 9. |
| `aspectRatio` | string | Não | '1:1' default. (1:1, 21:9, 16:9, 9:16, 4:3, 4:5, 5:4, 3:4, 3:2, 2:3) |
| `resolution` | string | Não | '2k' or '4k' (default). (2k, 4k) |
| `selectedAngles` | string[] | Não | Angles mode labels, ideally one per tile. |
| `selectedEthnicities` | string[] | Não | Demographic labels. |
| `selectedGenders` | string[] | Não | Demographics mode genders. (male, female) |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_library_create`

Create a reusable library asset (character/product/location) from 1-6 images. _(POST /api/magnific/library/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Sim | Unique A-Z/0-9/_/- name, max 50. |
| `type` | string | Sim | character=people/animals/avatars; product=objects; locations=places. (character, product, locations) |
| `description` | string | Não | Max 1000. |
| `gender` | string | Não | Character only. |
| `productType` | string | Não | Product only. |
| `images` | object[] | Sim | 1-6 JPEG/PNG/WebP images; first is cover. |

#### `magnific_library_list`

Lean library catalog (characters, styles, elements, locations) for agent reasoning. _(POST /api/magnific/library/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `scope` | string | Não | all (default)|projects|organization. (all, projects, organization) |
| `type` | string | Não | character|style|element|locations. (character, style, element, locations) |
| `page` | integer | Não | Default 1. |
| `perPage` | integer | Não | Default/max 50. |
| `search` | string | Não | Filter by name/description. |

#### `magnific_models3d_generate`

Image-to-3D GLB. Upload external images first. Animate with `models3d_rig`. Models: tripo-p1 default, tripo-v31, trellis-2. _(POST /api/magnific/models3d/generate)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Sim | Source image identifier. |
| `model` | string | Não | tripo-p1 (default, fast), tripo-v31 (HQ), trellis-2. (tripo-p1, tripo-v31, trellis-2) |
| `resolution` | integer | Não | trellis-2: 512, 1024 default, 1536. (512, 1024, 1536) |
| `textureQuality` | string | Não | Tripo: none, standard default, detailed v31 only. (none, standard, detailed) |
| `faceLimit` | integer | Não | Tripo faces. tripo-p1: 100..20,000 (default 10,000); tripo-v31: up to 2,000,000. Optional — omit for the model default. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_project_report`

English markdown status report for a project/folder. _(POST /api/magnific/project/report)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `reference` | string | Sim | Project/folder reference from `folders_list` or `folders_get`. |
| `days` | integer | Não | 1-30; default 7. |

#### `magnific_spaces_create`

Create empty Space. Share `space.webUrl`; only call `spaces_show` when inline preview is requested. _(POST /api/magnific/spaces/create)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Sim | Display name. |
| `description` | string | Não | Max 1000 chars. |

#### `magnific_spaces_edit`

External MCP only. Start headless Space edit. Poll `spaces_edit_status` until allTerminal, then verify with `spaces_state`. _(POST /api/magnific/spaces/edit)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `query` | string | Sim | Edit request. |
| `spaceId` | string | Sim | Space UUID. |
| `threadId` | string | Não | Legacy grouping id. |
| `selectedElementIds` | string[] | Não | Selected node ids. |
| `selectedConnectionIds` | string[] | Não | Selected connection ids. |
| `anchorElementId` | string | Não | Element to extend from. |
| `anchorDirection` | string | Não | Direction from anchorElementId. (left, right, up, down) |

#### `magnific_spaces_edit_status`

Wait for `spaces_edit` like `creations_wait`: long-polls up to `timeoutSeconds` (default 25, max 25). _(POST /api/magnific/spaces/edit/status)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `operationId` | string | Não | From `spaces_edit`; preferred. |
| `threadId` | string | Não | Legacy thread_id from `spaces_edit`. |
| `runId` | string | Não | Specific run_id. |
| `timeoutSeconds` | integer | Não | Long-poll seconds (default 25, max 25). 0 = snapshot only + poll_after_seconds hint. |

#### `magnific_spaces_get_nodes`

Read-only. Like `spaces_state` but scoped to `nodeIds` + their connections. Cheaper than full board. Needs view perm. _(POST /api/magnific/spaces/get/nodes)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `spaceId` | string | Sim | Space UUID. |
| `nodeIds` | string[] | Sim | Node ids to fetch. Returns these + all incoming/outgoing connections. |
| `currentPageId` | string | Não | Current page id for pageId resolution. |

#### `magnific_spaces_list`

Lean spaces catalog as raw TOON text. _(POST /api/magnific/spaces/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `query` | string | Não | Name filter. |
| `ownership` | string | Não | Default all. (all, mine, shared) |
| `page` | integer | Não | Default 1, max 1000. |
| `perPage` | integer | Não | Default 25, max 100. |

#### `magnific_spaces_run`

Run Space workflow from `startNodeId`. _(POST /api/magnific/spaces/run)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `spaceId` | string | Sim | Space UUID. |
| `startNodeId` | string | Sim | Node id from `spaces_state`. |
| `mode` | string | Não | Default connected. (singular, connected, downstream) |

#### `magnific_spaces_run_status`

Poll `spaces_run`. `timeoutSeconds` long-polls up to 25s. In-progress returns `poll_after_seconds`; terminal call `creations_show`. _(POST /api/magnific/spaces/run/status)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `workflowRunIdentifier` | string | Sim | Run identifier from `spaces_run`. |
| `timeoutSeconds` | integer | Não | 0 snapshot, 1-25 long-poll. |

#### `magnific_spaces_state`

Read-only board context, raw TOON. _(POST /api/magnific/spaces/state)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `spaceId` | string | Sim | Space UUID. |
| `selectedElementIds` | string[] | Não | Selected node ids. |
| `selectedConnectionIds` | string[] | Não | Selected connection ids. |
| `currentPageId` | string | Não | Page the user is viewing. |
| `pageId` | string | Não | Read a specific page; falls back to currentPageId. |
| `scope` | string | Não | current_page (default) | all (whole board, costly). |

#### `magnific_stock_download`

Signed download URL for a stock item. _(POST /api/magnific/stock/download)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `id` | integer | Sim | Freepik stock item ID. |
| `type` | string | Não | Default: photo. Use "icon" for SVG. (photo, vector, video, icon, psd) |
| `folder_reference` | string | Não | Project folder UUID. When provided, creation saved to library. |

#### `magnific_stock_get`

Full details for a single Freepik stock item: preview URL, dimensions, formats, author. _(POST /api/magnific/stock/get)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `id` | integer | Sim | Freepik stock item ID. |
| `type` | string | Não | Default: photo. Use "video" for videos, "icon" for SVG. (photo, vector, video, icon) |

#### `magnific_stock_search`

Search Freepik stock catalog by keyword. _(POST /api/magnific/stock/search)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `query` | string | Sim | Search keywords. |
| `content_type` | string | Não | Filter by type. Omit for all types. (photo, vector, video, icon) |
| `page` | integer | Não | Default: 1. |
| `per_page` | integer | Não | 1–50. Default: 20. |

#### `magnific_stock_to_creation`

Convert Freepik stock item → Pikaso creation. _(POST /api/magnific/stock/to/creation)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `id` | integer | Sim | Freepik stock item ID. |
| `folder_reference` | string | Não | Target project folder UUID. Omit to save without folder. |
| `title` | string | Não | Creation display title. Defaults to "Stock item {id}". |

#### `magnific_video_concatenate`

Concatenate completed video creations into one MP4. _(POST /api/magnific/video/concatenate)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `name` | string | Não | Label for the concatenated video in the media viewer. |
| `creationIdentifiers` | string[] | Sim | 2-10 completed video identifiers in playback order. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_video_generate`

Generate video. External clients: call `video_plan` first to draft the brief and resolve model choice — skip only if the user explicitly says "just generate" or "one-shot". Pick a model with `slug` (c _(POST /api/magnific/video/generate)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `video` | object | Sim |  |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_video_models_list`

Video-gen model catalog as lean TOON text. _(POST /api/magnific/video/models/list)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `search` | string | Não | Case-insensitive filter on name, slug, description, or tags. |
| `onlyRecommended` | boolean | Não | When true, returns only models with an `agentRecommendation`. |

#### `magnific_video_plan`

Call FIRST for any video request. _(POST /api/magnific/video/plan)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `prompt` | string | Sim | Raw user idea; pass verbatim. |
| `durationHint` | integer | Não | Seconds; >15 triggers multi-clip plan. |
| `styleHint` | string | Não | Visual style (e.g. "cinematic", "anime"). |
| `aspectRatioHint` | string | Não | Aspect ratio hint (e.g. "16:9"). |
| `referenceIdentifiers` | string[] | Não | Creation identifiers already attached as references. |

#### `magnific_video_speak`

Make a character speak. Image+audio → talking head (Veed Fabric 1.0). Video+audio → lip sync (Lipsync 2.0). Omit `mode` for auto. Pass asset URLs or a creation `identifier` (from `audio_tts`, `images_ _(POST /api/magnific/video/speak)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `audioUrl` | string | Sim | Asset URL or a creation `identifier` (from `audio_tts`). |
| `videoUrl` | string | Não | Asset URL or a creation `identifier`. Required without `imageUrl`; mutually exclusive with it. |
| `imageUrl` | string | Não | Asset URL or a creation `identifier`. Required without `videoUrl`; mutually exclusive with it. |
| `mode` | string | Não | Omit for auto: image → veed-fabric-1.0, video → lipsync-2.0. (latentsync, lipsync-2.0, omnihuman, veed-fabric-1.0, veed-fabric-1.0-fast, react-1) |
| `resolution` | string | Não | Only for veed modes; defaults to 720p. (480p, 720p) |
| `prompt` | string | Não | Script/prompt; model-dependent support. |
| `voiceId` | integer | Não | Voice ID from `audio_voices_list`; links the voice to the creation. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_video_upscale`

Upscale video via Topaz or Magnific. _(POST /api/magnific/video/upscale)_

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `creationIdentifier` | string | Não | Completed owned video identifier. Required without `videoUrl`. |
| `videoUrl` | string | Não | Freepik video URL. Required without `creationIdentifier`. |
| `mode` | string | Sim | See `video_upscale_models_list`. (topaz, magnific, magnific_precision) |
| `upscaleFactor` | number | Não | Topaz only, required: 1-8. |
| `targetFps` | number | Não | Topaz only, required: 0-60 FPS. |
| `targetResolution` | number | Não | Magnific/Precision required: width 640-3840. |
| `focus` | number | Não | Topaz only, required: 0-1. |
| `sharpen` | number | Não | Topaz: 0-1 required. Magnific: 0-100. |
| `enhancementModel` | string | Não | Topaz only, required. (proteus, artemis, nyx, rhea, gaia, colorize, dione, theia, iris, themis, starlightFast2, astra2, starlightPrecise25) |
| `frameInterpolation` | string | Não | Topaz only, required. (apollo, chronos) |
| `creativity` | number | Não | Magnific Creative only: 0-100. |
| `magnificResolution` | string | Não | Magnific preset. (720p, 1k, 2k, 4k) |
| `premiumQuality` | boolean | Não | Magnific Creative only. |
| `fpsBoost` | boolean | Não | Magnific/Precision only. |
| `smartGrain` | number | Não | Magnific/Precision: 0-100. |
| `flavor` | string | Não | Magnific Creative only. (vivid, natural) |
| `turbo` | boolean | Não | Magnific Creative only. |
| `preview` | boolean | Não | Magnific/Precision preview. |
| `strength` | number | Não | Precision 0-100. |
| `folderReference` | string | Não | Optional project/folder reference from `folders_list` to place results in. Omit for the default. |

#### `magnific_video_upscale_models_list`

Video upscale mode catalog as raw TOON text (topaz, magnific, magnific_precision) with required/optional params, ranges and enums. _(POST /api/magnific/video/upscale/models/list)_

---

Este MCP também funciona via **conexão MCP** (Claude / Cursor) em `https://api.mcp.ai/p_magnific` — veja o [README](../../README.md). A skill acima é pra consumir a **REST API** direto (agente próprio / código).
