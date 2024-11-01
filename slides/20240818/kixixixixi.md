# 機械学習を Typescript で扱いたい

2024-08-18

@kixixixixi

---

機械学習や統計処理をしなくていけないことが多いけれど、Python がきらいなので Typescript でかきたい

---

ブラウザで動かせればサーバーでの計算量を軽減したつくりにもできる

---

# sklearn

https://sklearn.vercel.app/

scikit-learn で使える機械学習が利用できる。
多くの分類・回帰などが利用可能。

仕組みとしては Python のラッパーであるのでローカルに Python の実行環境及びライブラリのインストールが必要

Install

```sh
pip install numpy scikit-learn
npm install sklearn
```

---

# sklearn で PCA

```ts
import * as sklearn from "sklearn";
(async () => {
  const py = await sklearn.createPythonBridge();
  const model = new sklearn.PCA({ n_components: 2 });
  await model.init(py);
  const result = await model.fit_transform({
    X: dataList,
  });
  await model.dispose();
  await py.disconnect();
  console.log(result);
})();
```

---

# sklearn

- 動作環境で Python の実行環境が必要

---

# ONNX

https://onnxruntime.ai/

Python などでつくった ONNX 形式の生成済みのモデルを実行できる

Install

```sh
npm install onnxruntime-web
```

---

# ONNX で線形判別

```ts
import { Tensor, InferenceSession } from "onnxruntime-web";
(async () => {
  const session = await InferenceSession.create("data/lda.onnx");
  const inputTensor = new Tensor("float32", data, [1, 40]);
  const results = await session.run({ input: inputTensor });
  console.log(results);
})();
```

---

# ONNX

- モデルが出力できる分析のみ利用可能
- トレーニングは Python などの環境で行う必要あり
- クロスプラットフォームで利用できる
- モデルを保存して実行する場合に Python でもよく使われる

---

# brain.js

Node.js 上や Web ブラウザ上で NN が可能

C++17 の問題？でインストールできない

---

# TensorFlow.js

https://www.tensorflow.org/js/

TensorFlow の JS 版
TesorFlow でできることが Node.js 上や Web ブラウザ上で可能に

---

# TensorFlow.js で TSNE

https://github.com/tensorflow/tfjs-tsne

型エラーで失敗

---

# ml5js

TensorFlow.js のラッパー
npm install が postinstall command でコケる

---

# Synaptic

node.js とブラウザの javascript アーキテクチャフリーニューラルネットワークライブラリ
