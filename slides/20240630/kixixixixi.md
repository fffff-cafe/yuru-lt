# 安野たかひろさんの AI/OSS 選挙

2024-06-30

@kixixixixi

---

# 安野たかひとさんとは

東京都知事候補

"安野(あんの)たかひろ は、AI エンジニア、起業家、SF 作家の経歴を持つ東京都知事候補者です。テクノロジーで誰も取り残さない東京を目指して本マニフェストを作成しています。"

https://takahiroanno.com/
https://gendai.media/articles/-/131469

---

# 参加型マニフェスト

「インターネット上で政策への改善・変更提案」ができる OSS の政策

https://note.com/takahiroanno/n/n38a34f107867

安野たかひろ：都知事選マニフェスト
https://github.com/takahiroanno2024/election2024

---

# 参加型マニフェスト

- Github Actions を Issues や Comment の投稿時に Hook
- text-embedding-3-small（OpenAI）で Embedding
- Qdrant で類似マニュフェストなど検索
- OpenAI の API で Text generate

mkdocs でマークダウンをドキュメント化

---

# 質疑応答システム 「AI あんの」

【都知事選 2024】AI によるマニフェストへの質疑応答システム「AI あんの」の裏側を公開します！
https://note.com/jujunjun110/n/n0362b324831f

"安野たかひろの政策を学習した AI 応答システムが、本人のアバターと声色によって、Youtube Live と電話という 2 つの経路で、みなさまのご意見やご質問に回答するシステム"
https://www.youtube.com/watch?v=WMcF6Ujrn2U

AI Vtuber ?

---

# 質疑応答システム 「AI あんの」

- Youtube コメントを取得
- Embedding
- faiss を使った RAG で該当の政策を取得
- OpenAI の Completion で回答生成
- VRM と Azure 音声読み上げで画像音声生成
- OBS で配信

---

# RAG

Retrieval-Augmented Generation (RAG) は、大規模言語モデル（LLM）によるテキスト生成に、外部情報の検索を組み合わせることで、回答精度を向上させる技術のこと

---

# RAG の実装やってみた

https://github.com/kixixixixi/rag-bot

---

# 技術構成

- Node.js
- Next.js
- Qdrant
- GluCoSE
- Gemini Pro

---

# デプロイ

API キーなどを秘匿化したいので、サーバーを立てる必要あり。

- Vercel
- Fly.io
