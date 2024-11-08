# 認証システムの切り離しをしたい

2024-11-03

@kixixixixi

---

# どういうことか

ユーザー認証の機能を Web アプリケーションから切り離して実装すること。

## Example

Google アカウントへのログインは OpenID Connect のプロトコルに準じた実装になっている。ログインしようとすると accounts.google.com へ飛び認証がされるとユーザーのプロフィール情報を含む ID トークンを発行して認証情報が共有される。

---

# メリット

- セキュリティ
  パスワード管理やセッション管理を専門のシステムに任せることで、セキュリティのリスクを低減
- メンテナンス負担の軽減
  認証部分のアップデートや改修が分離でき、メンテナンスが容易に
- 再利用性
  認証基盤が独立していると、他のアプリケーションやマイクロサービスとも統一した認証基盤を共有できる
- スケーラビリティ
  独立したサービスとして切り離すことで、認証のみをスケールさせることが容易に（認証処理はリソースを消費しやすい）

---

# デメリット

- インテグレーションの複雑さ
  認証基盤を外部に切り離すと、アプリケーション本体との統合が複雑になりやすい
  トークン管理やセッション共有の仕組みが必要
- パフォーマンスの影響
  認証処理において、本体アプリケーションと認証基盤の間でネットワーク通信が発生するため、通信遅延がパフォーマンスに影響
- 運用の複雑化
  認証基盤の管理が別途必要になるため、運用負荷が増える

---

# BaaS で実現する

BaaS（Backend as a Service）を利用することで、認証システムを含むバックエンドのインフラを簡素化できる。BaaS には認証機能が備わっているものがあり、迅速かつ安全に認証システムを導入できる。

認証システムの「車輪の再開発」は辞めるべき！

---

# BaaS で実現する

- 多くの認証方法が利用可
  メール認証や SNS 認証、SMS 認証が利用可能
- 他のサービスとの連携が容易
  外部認証を利用すると他の API やサービスと簡単に連携可能
- 多要素認証の導入が容易
  外部認証サービスには多要素認証（MFA）を提供しているものが多く、セキュリティ強化が容易
- ロックイン
  依外部サービスに依存することで利用停止やサービス終了などに影響をうける
- 費用
  有料の場合あり
- カスタマイズの制約
  サービスの仕様に縛られるため、細かなカスタマイズが難しい

---

# Firebase での実装

Firebase Authentication は、Google が提供する BaaS の一部で、パスワード認証やソーシャルログイン、メールリンク認証などが簡単に導入できる

- ユーザー情報の管理や認証ステートの管理が容易
- サーバーレス環境での迅速なセットアップが可能

---

```js
import { onAuthStateChanged } from "firebase/auth";

onAuthStateChanged(auth, async (user) => {
  setFirebaseUser(user);
  if (!user) {
    return;
  }
  const token = await user?.getIdToken();
});

import { getAuth, browserLocalPersistence } from "firebase/auth";

const firebaseConfig = { ... };

const app =
  getApps().length === 0 ? initializeApp(firebaseConfig) : getApps()[0];

const auth = getAuth(app);
auth.setPersistence(browserLocalPersistence);

const { user } = await signInWithEmailAndPassword(auth, email, password);
```

---

# Supabase での実装

Supabase はオープンソースの Firebase 代替サービスで、PostgreSQL ベースのデータベースを提供しているため、SQL の強力なデータ管理と組み合わせて利用できる

- 低コストでオープンソースなので、柔軟なカスタマイズが可能
- RESTful API の自動生成やリアルタイム機能を持つため、モダンな Web アプリケーションに適した構成が可能

```js
export const supabase = createClient( ... )
const { data: { user }, error } = await supabase.auth.getUser()

// const { error } = await supabase.auth.signInWithOAuth({ provider: "github" });
await signInWithPassword({email, password})

const { data: { session: { access_token } } } = await supabase.auth.getSession()
```

onAuthStateChange もある

---

# 課題

クラウド指定される案件とか
