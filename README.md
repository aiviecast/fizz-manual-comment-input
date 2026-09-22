# fizz-manual-comment-input

**Fizz** コメント入力系部品 — 運用者が stdin に打った行を正規化コメント
([fizz-protocol](https://github.com/aiviecast/fizz-protocol) の `Comment`、
source = `Manual`) として NDJSON で stdout に流す。

責務は一行: **運用者の手入力 → comment ストリーム**。
テスト注入・リハーサル・配信中の手動コメントに使う。

## Usage

```bash
almide build src/main.almd -o fizz-manual-comment-input

# 対話的に
./fizz-manual-comment-input | fizz-comment-dedupe | ...

# ワンショット注入
echo "こんにちは" | ./fizz-manual-comment-input
echo "常連さん	また来たよ" | ./fizz-manual-comment-input   # TAB 区切りで user 指定
```

## 入力形式

| 入力 | user | text |
|---|---|---|
| `こんにちは` | `FIZZ_MANUAL_USER` (default: `operator`) | こんにちは |
| `常連さん<TAB>また来たよ` | 常連さん | また来たよ |
| 空行 | — | **EOF として終了** (read_line の制約で空行と EOF を区別できない) |

ID は `man-<unix_ms>-<seq>` で採番。

## Env

| var | 意味 | default |
|---|---|---|
| `FIZZ_MANUAL_USER` | user 未指定時の名前 | `operator` |

## Tests

```bash
almide test
```
