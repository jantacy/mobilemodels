# Phone Brand & Model Summary

Phone manufacturers often expose only a raw, cryptic model code (e.g. `MI-ONE PLUS`) at the OS/analytics level instead of the human-readable name (e.g. "小米 1 联通版"). [KHwang9883's MobileModels](https://github.com/KHwang9883/MobileModels) crowdsources the mapping between these codes and their real names, one markdown file per brand.

This repo pulls that project in as a git submodule, parses every brand's markdown file, and flattens the result into a single CSV — so you can just load one file instead of parsing 20+ markdown files yourself.

## Output

[brand_model.csv](https://github.com/jantacy/mobilemodels/blob/main/brand_model.csv) — one row per device, with columns:

| Column | Description |
|---|---|
| `brand` | Brand slug (e.g. `xiaomi`, `huawei`, `apple`) |
| `model` | Raw model code as reported by the device (e.g. `MI-ONE PLUS`) |
| `area` | `cn` or `en`, depending on which regional markdown file the entry came from |
| `brand_name` | Brand name in Chinese (e.g. `小米`) |
| `model_name` | Human-readable model name (e.g. `小米 1 联通版`) |

## Auto-update

A GitHub Actions workflow ([`update.yml`](.github/workflows/update.yml)) runs daily:

1. Pulls the `MobileModels` submodule to its latest commit
2. Regenerates `brand_model.csv` by running [`phoneModel.py`](phoneModel.py)
3. Commits and pushes if the data changed

No manual token setup is needed — it authenticates with the built-in `GITHUB_TOKEN`. If you fork this repo and the push step fails with a permissions error, check Settings → Actions → General → Workflow permissions and set it to "Read and write permissions".

## Running locally

```bash
git clone --recurse-submodules https://github.com/jantacy/mobilemodels.git
cd mobilemodels
pip install -r requirements.txt
python phoneModel.py
```
