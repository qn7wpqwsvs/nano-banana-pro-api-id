# Nano Banana Pro API (`gemini-3-pro-image-preview`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.03 per image** (4K $0.04) — satu endpoint yang kompatibel dengan OpenAI di `https://api.apimart.ai/v1`, tanpa paket bulanan. *(diamati 2026-10-12)*

**[Dapatkan API key](https://go.apimart.ai/k-ac7503)** · **[Harga live](https://go.apimart.ai/k-b7c5ef)** · **[Halaman model](https://go.apimart.ai/k-2cc762)** · [⚡ Quickstart 60 detik](#quickstart)

**Alasan tim memakai Nano Banana Pro (`gemini-3-pro-image-preview`) lewat APIMart**

- **Satu key untuk seluruh katalog.** base URL `https://api.apimart.ai/v1` dan header `Authorization` yang sama menjangkau Nano Banana Pro (`gemini-3-pro-image-preview`) plus 300+ model gambar, video, dan bahasa lainnya — cukup ganti field `model`, bukan klien Anda.
- **Minimum $1, bayar sesuai pemakaian.** Tanpa langganan dan tanpa paket prabayar yang harus dihitung di awal: isi saldo mulai $1 dan pakai untuk panggilan.
- **Biaya dikembalikan di respons.** Setiap task yang selesai melaporkan jumlah yang ditagih (`cost` / `credits_cost`); task berstatus `failed` dikembalikan dananya.
- **Asinkron sejak awal.** Kirim, ambil `task_id`, polling `GET /v1/tasks/{id}` — batching dan retry hanyalah pekerjaan antrean biasa, bukan integrasi khusus.

<!-- /conv-kit:v1 -->



Nano Banana Pro is Google's Gemini 3 Pro Image for 4K image generation and editing with multi-step planning.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `gemini-3-pro-image-preview` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-2cc762) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.0375 | 0.03 |
| 4K | 0.05 | 0.04 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-b7c5ef) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-2cc762) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"gemini-3-pro-image-preview","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "gemini-3-pro-image-preview", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Apa id model Nano Banana Pro?**

`gemini-3-pro-image-preview`. Lihat tabel rute di atas.

**Bagaimana penagihannya?**

**from $0.03 per image** (4K $0.04). Task yang selesai melaporkan jumlah tepatnya di field `cost`; task yang gagal dikembalikan dananya.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-ac7503) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [nano-banana-pro-prompts-id](https://github.com/qn7wpqwsvs/nano-banana-pro-prompts-id)
- **Same model, other languages** — 9 more language builds: 中文 [nano-banana-pro-api-cn](https://github.com/j2pnwx3z8juf/nano-banana-pro-api-cn) · Deutsch [nano-banana-pro-api-de](https://github.com/rs9td1w5p7e/nano-banana-pro-api-de) · Español [nano-banana-pro-api-es](https://github.com/g6-erqods5/nano-banana-pro-api-es) · Français [nano-banana-pro-api-fr](https://github.com/sgta2m/nano-banana-pro-api-fr) · Relay/gateway `nano-banana-pro-api-gateway` · 日本語 [nano-banana-pro-api-ja](https://github.com/z9wa-4xsxvrp/nano-banana-pro-api-ja) · 한국어 [nano-banana-pro-api-ko](https://github.com/wompd2anc/nano-banana-pro-api-ko) · Português [nano-banana-pro-api-pt](https://github.com/f0xtn9oh8g7ia6/nano-banana-pro-api-pt) · Русский [nano-banana-pro-api-ru](https://github.com/v4l5npww2/nano-banana-pro-api-ru)
- **More models in this language** — [midjourney-api-id](https://github.com/cvrwq0/midjourney-api-id) · [gemini-2-5-flash-image-preview-api-id](https://github.com/gmrg0s/gemini-2-5-flash-image-preview-api-id) · [nano-banana-api-id](https://github.com/vsa185x9ecrd/nano-banana-api-id) · [gemini-3-1-flash-image-preview-api-id](https://github.com/p61qrgpg/gemini-3-1-flash-image-preview-api-id) · [nano-banana-2-api-id](https://github.com/n1rf4ezzz/nano-banana-2-api-id) · [gemini-3-pro-image-preview-api-id](https://github.com/s78ve5v4x9ji2/gemini-3-pro-image-preview-api-id)
- **Topic hubs** — every build of this model, all languages: [nano-banana-pro](https://github.com/topics/nano-banana-pro) · [nano-banana-pro-api](https://github.com/topics/nano-banana-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-2cc762> | `apimart.ai/id/model/nano-banana-2-api` |
| Live pricing | <https://go.apimart.ai/k-b7c5ef> | `apimart.ai/id/pricing` |
| Get an API key | <https://go.apimart.ai/k-ac7503> | `apimart.ai/id/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Nano Banana Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
