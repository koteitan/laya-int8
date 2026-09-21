# laya-int8

[mizchi/laya-multilingual-onnx](https://huggingface.co/mizchi/laya-multilingual-onnx)
(float16, 647 MB) を int8 に動的量子化したもの。**325 MB**。

[laya-bot-det](https://github.com/koteitan/laya-bot-det) で、WebKit が
647 MB のモデルを `InferenceSession.create` の中で読めずタブごと落ちる問題を、
サイズを半分にすれば回避できるか試すために用意した。

```
https://koteitan.github.io/laya-int8/
```

## 分割

GitHub は 1 ファイル 100 MB を超えると受け付けないので、`model.onnx` は
4 つに分けてある。`model.onnx.parts.json` が繋ぎ方を持つ。

```json
{ "file": "model.onnx", "size": 325013504, "sha256": "...",
  "parts": [{ "name": "model.onnx.000", "size": 83886080 }, ...] }
```

laya-bot-det の `web/src/laya/download.ts` は、この manifest があれば各パートを
Range 取得して繋ぎ直す。分割されていない通常のバンドルもそのまま読める。

## 精度

元の float16 と比べて AUC はほぼ同じだが、**author ごとの順位はかなり食い違う**
(自己申告ラベル 46 件で Spearman 0.584、p_bot の平均絶対差 0.151)。
軽い代替品ではなく、別の判定器として扱うこと。

| | Laya AUC | 合成 AUC |
|---|---|---|
| float16 | 0.706 | 0.842 |
| int8 | 0.735 | 0.831 |

## ライセンス

Apache-2.0。派生の連鎖は NOTICE を見ること。
