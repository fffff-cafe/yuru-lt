# 地図データで遊ぶ

2024-05-19

@kixixixixi

---

# 地図データをつかったサービスをつくりたい

---

# Google map

---

# Google map を使う

https://www.google.com/intl/ja/help/terms_maps/

禁止されている

- Google マップ / Google Earth の一部を再配布もしくは販売すること
- コンテンツをコピーすること
- Google マップ / Google Earth を使用して地図関連の別のデータセットを作成すること

それに Google map API は高い

---

# 安心して使える地図

- OpenStreetMap
- 地理院地図

---

# 地理院タイル

国土地理院が配信するタイル状の地図データ
出典元を記載すれば利用可能！

![w:300](https://maps.gsi.go.jp/help/image/data_providing.png)

---

# 利用法

以下の形式でタイル画像が取得できる
https://cyberjapandata.gsi.go.jp/xyz/{t}/{z}/{x}/{y}.{ext}

- {t}：データ ID
- {x}：タイル座標の X 値
- {y}：タイル座標の Y 値
- {z}：ズームレベル
- {ext}：拡張子

---

# 緯度経度からタイル座標に

西経 180 度、北緯約 85.0511 度の北西端を左上の端点にもつタイルを(0,0)として東方向を X 正方向、南方向を Y 正方向

![w:400](https://maps.gsi.go.jp/help/image/tileNum.png)

---

# 変換

```js
export const fromGeoPositionToPoint = ({
  position,
  z,
}: {
  position: GeolocationPosition
  z: number
}): { x: number; y: number; z?: number } => {
  return {
    x: 2 ** (z + 7) * (position.coords.longitude / 180 + 1),
    y:
      (2 ** (z + 7) / Math.PI) *
      (-Math.atanh(Math.sin((Math.PI / 180) * position.coords.latitude)) +
        Math.atanh(Math.sin((Math.PI / 180) * 85.05112878))),
    z,
  }
}
```

---

# 例

変換サイト
https://maps.gsi.go.jp/development/tileCoordCheck.html#5/35.362/138.731

ここの地図タイル
https://cyberjapandata.gsi.go.jp/xyz/std/18/232848/103215.png

![](https://cyberjapandata.gsi.go.jp/xyz/std/18/232848/103215.png)

使えるタイル
https://maps.gsi.go.jp/development/ichiran.html

---

# ブラウザで緯度経度を取得

```js
navigator.geolocation.getCurrentPosition((position) => {
  // position: GeolocationPosition
});
```

非同期的に取得可能

---

# 表示してみた

https://kixixixixi.github.io/map-sandbox/

---

# 実装してみて

ズームやドラッグでの移動などが面倒...

---

# Leaflet

タイル画像に対応した UI ライブラリ
https://leafletjs.com/
https://github.com/Leaflet/Leaflet

React 対応
https://react-leaflet.js.org/
https://github.com/PaulLeCam/react-leaflet

---

# 実装してみた

https://github.com/kixixixixi/leaflet-sandbox
