# Magnific

### Magnific para Claude, ChatGPT e agentes de IA

Magnific por linguagem natural: faça upscale de imagens, gere e liste suas criações. Conecte com sua conta Magnific e use os créditos do seu plano.

- 📊 **61 ferramentas**
- ✏️ **Leitura e escrita**
- 💬 **Funciona com qualquer cliente MCP**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Login via magic-link (sem senha)**

[English version](README.en.md) · [Documentação completa](docs/) · [Skill pra agentes](skills/)

---

## Instalar em 1 clique

### Claude (Web e Desktop)

A Anthropic unificou a instalação de MCPs em `claude.ai/customize/connectors`. **O mesmo link serve pra Claude Web e Claude Desktop** (basta estar logado):

[➕ Abrir no Claude e conectar](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors)

**Manual** (se o deeplink não abrir): [claude.ai/customize/connectors](https://claude.ai/customize/connectors?surface=cowork) → **+** → **Adicionar conector personalizado** → cole **Nome** `Magnific` e **URL** `https://api.mcp.ai/p_magnific`.

### Cursor

[➕ Instalar Magnific no Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=magnific&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9tYWduaWZpYyJ9)

### VS Code (Copilot Chat)

[➕ Instalar Magnific no VS Code](vscode:mcp/install?name=magnific&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_magnific%22%7D)

### ChatGPT, Manus, OpenClaw e mais 40+ clientes

Funciona em qualquer cliente MCP que suporte **MCP over HTTP**. A URL do servidor é sempre:

```
https://api.mcp.ai/p_magnific
```

Detalhes por cliente: [INSTALL.md](INSTALL.md).

---

## Exemplos de uso

```
Faça upscale dessa imagem no Magnific
Liste minhas criações recentes no Magnific
Gere uma variação dessa imagem no Magnific
```

---

## 61 ferramentas disponíveis

| Tool | Descrição |
|---|---|
| `magnific_account_balance` | User plan + credits. Check before paid generations. Returns plan.{tier,productName,isUnlimitedMode}, credits.{available,totalPlan,spent,hasExtraCredits}. No history. |
| `magnific_creations_wait` | Long-poll 1..8 creations until terminal or `timeoutSeconds` (default 25, max 25). |
| `magnific_creations_get` | Single creation complete data — metadata + URLs (`url` full-res, `previewUrl` ~1024px, `thumbnailUrl` ~400px). |
| `magnific_creations_search` | Search user creations as raw TOON text. |
| `magnific_creations_list` | Filterable gallery widget (image/video/audio). |
| `magnific_creations_request_upload` | Step 1 for local/user files: get presigned PUT URL(s). |
| `magnific_creations_finalize_upload` | Step 2 after PUT from `creations_request_upload`: convert temp path(s) to creations. |
| `magnific_creations_upload_image` | Upload public image URL in one step. |
| `magnific_creations_upload_file` | Import a file the user attached in the host (e.g. |
| `magnific_creations_move` | Move creations into `targetFolderReference`. |
| `magnific_creations_like` | Toggle heart/favorite reaction on a user-owned creation. |
| `magnific_creations_comment` | Add a comment (or reply) to a user-owned creation. |
| `magnific_folders_list` | List folders/projects as raw TOON text. |
| `magnific_folders_get` | Fetch folder metadata by `reference`. |
| `magnific_folders_create` | Create folder/project. No `parentReference`: workspace root. `type` defaults: project at root, folder when nested. |
| `magnific_folders_rename` | Rename a folder by `reference`. |
| `magnific_folders_delete` | Move a folder and its files to trash. |
| `magnific_project_report` | English markdown status report for a project/folder. |
| `magnific_spaces_list` | Lean spaces catalog as raw TOON text. |
| `magnific_spaces_create` | Create empty Space. Share `space.webUrl`; only call `spaces_show` when inline preview is requested. |
| `magnific_spaces_state` | Read-only board context, raw TOON. |
| `magnific_spaces_get_nodes` | Read-only. Like `spaces_state` but scoped to `nodeIds` + their connections. Cheaper than full board. Needs view perm. |
| `magnific_spaces_run` | Run Space workflow from `startNodeId`. |
| `magnific_spaces_run_status` | Poll `spaces_run`. `timeoutSeconds` long-polls up to 25s. In-progress returns `poll_after_seconds`; terminal call `creations_show`. |
| `magnific_spaces_edit` | External MCP only. Start headless Space edit. Poll `spaces_edit_status` until allTerminal, then verify with `spaces_state`. |
| `magnific_spaces_edit_status` | Wait for `spaces_edit` like `creations_wait`: long-polls up to `timeoutSeconds` (default 25, max 25). |
| `magnific_images_generate` | Generate/edit images. No refs=TTI. references[] (max 12; see field docs). Defaults: mode=auto, ratio=1:1, count<=8. resolution/quality per images_models_list. UI clients: after generating, MUST call `creations_show` w… |
| `magnific_images_remove_background` | Cut-out subject on transparent PNG. |
| `magnific_images_upscale` | AI upscale 2x/4x (Magnific, premium). |
| `magnific_images_skin_enhancer` | AI skin enhancer. faithful preserves identity; creative reinterprets; flexible uses presets. Defaults: faithful, sharpen=0, smartGrain=0. |
| `magnific_images_relight` | AI relight. 1-4 lights with azimuth/elevation enums, type neutral|gel, intensity 1-10. Gel uses hex `color`. |
| `magnific_images_change_camera` | Reframe camera around subject. |
| `magnific_images_variations` | Generate image-variation grid. |
| `magnific_images_crop` | Center-crop to `aspectRatio`. Pixels: `images_resize`. AI upscale: `images_upscale`. Ratios: 1:1, 16:9, 9:16, 4:3, 3:4, 3:2, 2:3, 21:9. |
| `magnific_images_resize` | Resize to exact pixels (no AI). |
| `magnific_images_to_svg` | Trace raster creation into SVG. |
| `magnific_images_generate_svg` | Text-to-SVG (Recraft v4 Pro Vector). |
| `magnific_images_models_list` | TTI model catalog as lean TOON text. |
| `magnific_library_list` | Lean library catalog (characters, styles, elements, locations) for agent reasoning. |
| `magnific_library_create` | Create a reusable library asset (character/product/location) from 1-6 images. |
| `magnific_models3d_generate` | Image-to-3D GLB. Upload external images first. Animate with `models3d_rig`. Models: tripo-p1 default, tripo-v31, trellis-2. |
| `magnific_video_plan` | Call FIRST for any video request. |
| `magnific_video_generate` | Generate video. External clients: call `video_plan` first to draft the brief and resolve model choice — skip only if the user explicitly says "just generate" or "one-shot". Pick a model with `slug` (copy it verbatim f… |
| `magnific_video_models_list` | Video-gen model catalog as lean TOON text. |
| `magnific_video_upscale` | Upscale video via Topaz or Magnific. |
| `magnific_video_upscale_models_list` | Video upscale mode catalog as raw TOON text (topaz, magnific, magnific_precision) with required/optional params, ranges and enums. |
| `magnific_video_concatenate` | Concatenate completed video creations into one MP4. |
| `magnific_video_speak` | Make a character speak. Image+audio → talking head (Veed Fabric 1.0). Video+audio → lip sync (Lipsync 2.0). Omit `mode` for auto. Pass asset URLs or a creation `identifier` (from `audio_tts`, `images_generate`, `video… |
| `magnific_audio_tts` | TTS voiceover. Single (voiceId) or 2-speaker (speakers). ElevenLabs/Google. No voiceId? Call `audio_voices_show` first (or `audio_voices_list` for agent-only lookup). |
| `magnific_audio_music_generate` | AI music generation. Google Lyria or ElevenLabs. Pass `prompt`, optional `model`/`durationSeconds`/`instrumental`. Lyria/Lyria-3 produce fixed 30s; Lyria-3-Pro accepts 30–180s; ElevenLabs accepts 10–300s. |
| `magnific_audio_voices_list` | TTS voice catalog as raw TOON text, no UI. |
| `magnific_design_auto_resize` | Create resized design pages from an image. |
| `magnific_design_auto_layers` | Extract editable layers from a flat image into a Pikaso design. |
| `magnific_flows_list` | Flows catalog as raw TOON text, no UI. |
| `magnific_flows_get` | Flow input/output spec as raw TOON text. |
| `magnific_flows_run` | Run flow. `flows_get` first for input specs. `inputs` is `{inputId: value}` (creation inputs from `creations_list`, voices from `audio_voices_show`). Returns `workflowRunIdentifier` → `flows_wait` → `creations_show`. |
| `magnific_flows_wait` | Long-poll a `flows_run`. Terminal: one entry per output creation with real `identifier` for `creations_show`. In-progress: `poll_after_seconds`. |
| `magnific_stock_search` | Search Freepik stock catalog by keyword. |
| `magnific_stock_get` | Full details for a single Freepik stock item: preview URL, dimensions, formats, author. |
| `magnific_stock_download` | Signed download URL for a stock item. |
| `magnific_stock_to_creation` | Convert Freepik stock item → Pikaso creation. |

Detalhe de cada tool: [docs/ferramentas.md](docs/ferramentas.md)

---

## Preços

Grátis.

---

## Privacidade & LGPD

- **Sub-processadores**: Magnific (Freepik), o LLM host que você escolher (Claude, ChatGPT, Cursor, agente próprio). Lista completa em [docs/privacidade-lgpd.md](docs/privacidade-lgpd.md).
- Os dados retornados pelas tools são enviados ao **LLM host que você escolher**, sub-processador fora do nosso controle. Recomendamos planos com opt-out de treinamento.

---

## Perguntas frequentes

**O servidor é open source?**
O servidor é proprietário (hosted). Este repositório é o wrapper público com manifestos, docs e skills — tudo MIT.

**Posso usar com agente próprio (não Claude/Cursor)?**
Sim — qualquer cliente que suporte MCP over HTTP. URL: `https://api.mcp.ai/p_magnific`.


---

## Suporte

- 📧 [magnific@mcp.ai](mailto:magnific@mcp.ai)
- 🐛 [GitHub Issues](https://github.com/mcp-dir/magnific-mcp/issues)
- 📄 [docs/](docs/)

---

## Licença

MIT — veja [LICENSE](LICENSE). O servidor MCP em `api.mcp.ai/p_magnific` é proprietário (hosted); este repositório (manifestos, docs, skills) é MIT.
