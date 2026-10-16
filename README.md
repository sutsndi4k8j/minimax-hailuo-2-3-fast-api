# MiniMax Hailuo 2.3 Fast API (`MiniMax-Hailuo-2.3-Fast`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0248 per second** on the APIMart route — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-d8ad52)** · **[Live pricing](https://go.apimart.ai/k-c1fdbf)** · **[Model page](https://go.apimart.ai/k-af76c3)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call MiniMax Hailuo 2.3 Fast (`MiniMax-Hailuo-2.3-Fast`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach MiniMax Hailuo 2.3 Fast (`MiniMax-Hailuo-2.3-Fast`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



MiniMax Hailuo 2.3 Fast on APIMart is the latest generation AI video model, offering superior visual quality, high motion coherence, and professional-grade performance. This page uses MiniMax-Hailuo-2.3-Fast for image-to-video generation and requires a first-frame image.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `MiniMax-Hailuo-2.3-Fast` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-af76c3) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| default | 0.031 | 0.0248 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-c1fdbf) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-af76c3) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"MiniMax-Hailuo-2.3-Fast","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "MiniMax-Hailuo-2.3-Fast", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the MiniMax Hailuo 2.3 Fast model id?**

`MiniMax-Hailuo-2.3-Fast`. See the route table above.

**How is it billed?**

**from $0.0248 per second** on the APIMart route. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-d8ad52) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [minimax-hailuo-2-3-fast-prompts](https://github.com/sutsndi4k8j/minimax-hailuo-2-3-fast-prompts)
- **Same model, other languages** — 10 more language builds: 中文 `minimax-hailuo-2-3-fast-api-cn` · Deutsch `minimax-hailuo-2-3-fast-api-de` · Español `minimax-hailuo-2-3-fast-api-es` · Français `minimax-hailuo-2-3-fast-api-fr` · Relay/gateway `minimax-hailuo-2-3-fast-api-gateway` · Bahasa `minimax-hailuo-2-3-fast-api-id` · 日本語 `minimax-hailuo-2-3-fast-api-ja` · 한국어 `minimax-hailuo-2-3-fast-api-ko` · Português `minimax-hailuo-2-3-fast-api-pt` · Русский `minimax-hailuo-2-3-fast-api-ru`
- **More models in this language** — [grok-image-api](https://github.com/hpcsd0apt/grok-image-api) · [seedream-4-0-api](https://github.com/qela34r8sb7/seedream-4-0-api) · [wan-2-7-image-pro-api](https://github.com/u3cdzay/wan-2-7-image-pro-api) · [z-image-api](https://github.com/t917b/z-image-api) · [minimax-h3-api](https://github.com/apimartchlp3td/minimax-h3-api) · [minimax-hailuo-02-api](https://github.com/hnqqarqc5jlt/minimax-hailuo-02-api)
- **Topic hubs** — every build of this model, all languages: [minimax-hailuo-2-3-fast](https://github.com/topics/minimax-hailuo-2-3-fast) · [minimax-hailuo-2-3-fast-api](https://github.com/topics/minimax-hailuo-2-3-fast-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-af76c3> | `apimart.ai/model/minimax-hailuo-2-3-fast` |
| Live pricing | <https://go.apimart.ai/k-c1fdbf> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-d8ad52> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

MiniMax Hailuo 2.3 Fast is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
