# Indic BFCL: changes in this copy

This repository is the Berkeley Function Calling Leaderboard (BFCL) from
[ShishirPatil/gorilla](https://github.com/ShishirPatil/gorilla), specifically its
`berkeley-function-call-leaderboard/` folder at commit `6ea5797`, which is the root
here. The other projects in the gorilla repository are not included. On top of it are the small changes needed to run the
[Indic BFCL](https://huggingface.co/datasets/ai4bharat/indic-bfcl) translations of BFCL v4
(14 Indian languages). Everything else is upstream, unchanged.

## What changed

| file | change |
|---|---|
| `bfcl_eval/constants/eval_config.py` | The data directory comes from `BFCL_DATA_PATH` (default: the bundled English `data/`). The language is read from the directory name (`data_hi` → `hi`), or from `BFCL_LOCALE`. |
| `bfcl_eval/constants/default_prompts.py`, `bfcl_eval/utils.py` | For non-English data, the memory and web_search categories add one sentence to the system prompt: answer the user in their language, but write the `answer` field in English, because those categories are graded by substring match against an English answer. English runs do not get this sentence, so English scores match the official leaderboard. |
| `.gitignore`, `LICENSE` | Upstream's root `.gitignore` (BFCL part) and Apache-2.0 licence, carried over because this folder is now the repository root. Also ignores `bfcl_eval/data_*/`. |

There are no changes to the checkers, to the model handlers, or to how scores are computed.

## Running a language

```bash
pip install -e .
huggingface-cli download ai4bharat/indic-bfcl --repo-type dataset --local-dir indic-bfcl

export BFCL_DATA_PATH=$PWD/indic-bfcl/data_hi
bfcl generate --model <model> --result-dir result_hi
bfcl evaluate --model <model> --result-dir result_hi --score-dir score_hi
python indic-bfcl/tools/bfcl_breakdown.py --result-dir result_hi/<model> \
    --score-dir score_hi/<model> --burden indic-bfcl/burden/hi.json
```
