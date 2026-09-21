# sizetest

構造が最小（ノード 1 個）で、サイズだけ違う ONNX。

laya-bot-det が WebKit で `InferenceSession.create` の中でタブごと落ちる原因を
切り分けるために作った。Laya は数千ノードあるが、これは 1 ノードなので、
**バイト数だけを変数にできる**。

| ファイル | サイズ |
|---|---|
| `size32.onnx` | 34 MB |
| `size64.onnx` | 67 MB |
| `size128.onnx` (2 分割) | 134 MB |
| `size192.onnx` (3 分割) | 201 MB |
| `size256.onnx` (4 分割) | 268 MB |
| `size325.onnx` (5 分割) | 341 MB |

100 MB を超えるものは GitHub の 1 ファイル上限に合わせて分割してある。
`<name>.parts.json` が繋ぎ方を持つ。

## これまでの測定 (Safari 26.6.1 / WebKit)

- 34 MB: 通る (create 2.5 秒)
- 341 MB: タブごと落ちる

ノード数は同じなので、原因はグラフの複雑さではなくバイト数。
残りのサイズで壁の位置を挟み込む。
