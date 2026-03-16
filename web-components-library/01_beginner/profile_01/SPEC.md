# SPEC.md — プロフィールカード

> 制作者・開発者向けの実装仕様書です。
> `SPEC.md` は **private リポジトリのみ** に含めてください。public リポジトリには push しないこと。

---

## 設計情報

| 項目 | 内容 |
|---|---|
| 難易度 | ★☆☆☆（初級） |
| 使用技術 | HTML / CSS |
| JS 依存 | なし |
| バリエーション | 3種類（シンプル / 横並び / ダーク） |
| 対応ブラウザ | Chrome / Safari / Edge / Firefox 最新版 |
| SP 対応 | あり（640px 以下で自動調整） |

---

## ファイル構成

```
01_profile-card/
├── README.md     ← クライアント向け説明
├── SPEC.md       ← 制作者向け実装仕様（本ファイル）
├── design.html   ← デザイン仕様書（クライアント確認用）
└── index.html    ← 実装・動作確認
```

---

## 習得ポイント

- `display: flex` + `align-items: center` + `justify-content: center` による中央寄せ
- `border-radius: 50%` でアバターを円形に
- `border-radius: 16px` + `box-shadow` でカードの立体感
- `:hover` + `transition: transform .2s ease` でカードの浮き上がりアニメーション
- `::before` 疑似要素でダークカードの背景装飾（Variation C）

---

## クラス設計

### Variation A — `.card-simple`（縦型・シンプルカード）

```
.card-simple
├── .avatar           円形アバター（88 × 88px）
├── .name             氏名
├── .role             肩書（アクセントカラー）
├── .bio              説明文（最大3行推奨）
├── .skills           スキルタグのFlexラッパー
│   └── .skill-tag    個別タグ（ピル型）
└── .cta-btn          CTAボタン（width: 100%）
```

### Variation B — `.card-horizontal`（横並び・スタッフ一覧）

```
.card-horizontal      display: flex
├── .avatar           円形アバター（72 × 72px・flex-shrink: 0）
└── .info             テキストエリア（flex: 1）
    ├── .name
    ├── .role
    ├── .bio
    └── .social
        └── .social-btn   SNSリンク（28 × 28px）
```

### Variation C — `.card-dark`（ダーク・実績付き）

```
.card-dark            background: #1a1a2e / overflow: hidden
├── ::before          装飾円（右上・疑似要素）
├── .avatar           circle + border: 3px solid アクセントカラー
├── .name
├── .role
├── .divider          区切り線（width: 40px）
├── .bio
└── .stats            display: flex
    └── div × n
        ├── .stat-num     数値（22px / Bold）
        └── .stat-label   ラベル（10px）
```

---

## デザイントークン

| 役割 | 値 |
|---|---|
| Avatar Gradient A | `linear-gradient(135deg, #667eea, #764ba2)` |
| Avatar Gradient B | `linear-gradient(135deg, #f093fb, #f5576c)` |
| Accent / Role Color | `#667eea` |
| Skill Tag Background | `#f0f0ff` |
| Dark Card Background | `#1a1a2e` |
| Body Text | `#1a1a2e` |
| Muted Text | `#888888` |
| Card Border Radius | `16px` |
| Shadow（Default） | `0 4px 24px rgba(0,0,0,.08)` |
| Shadow（Hover） | `0 12px 32px rgba(0,0,0,.12)` |
| Transition | `.2s ease` |

---

## インタラクション仕様

| 状態 | プロパティ | 値 |
|---|---|---|
| Default | `box-shadow` | `0 4px 24px rgba(0,0,0,.08)` |
| Hover（カード） | `transform` | `translateY(-6px)` |
| Hover（カード） | `box-shadow` | `0 12px 32px rgba(0,0,0,.12)` |
| Hover（ボタン） | `background` | `#5a6fd6`（darkened） |
| Hover（ボタン） | `transform` | `translateY(-1px)` |

```css
/* transition は all を使わず個別指定（パフォーマンス対策） */
.card-simple {
  transition: transform .2s ease, box-shadow .2s ease;
}
```

---

## レスポンシブ仕様

```css
/* 640px 以下（SP） */
@media (max-width: 640px) {

  /* 縦型カード：幅を全幅に */
  .card-simple {
    width: 100%;
  }

  /* 横型カード：縦並びに切り替え */
  .card-horizontal {
    flex-direction: column;
    align-items: center;
    text-align: center;
  }
}
```

**一覧グリッドで複数並べる場合**

```css
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 24px;
}
/* カード数・画面幅に関わらず自動でカラム数を調整 */
```

---

## カスタマイズポイント

```css
/* ① .avatar — グラデーションをブランドカラーに変更 */
.avatar {
  background: linear-gradient(135deg, #[色1], #[色2]);
}

/* ② .skill-tag — テキスト色と背景色をブランドカラーに変更 */
.skill-tag {
  color: #[ブランドカラー];
  background: #[薄い色];
}

/* ③ .cta-btn / .btn — ボタン色をブランドカラーに変更 */
.cta-btn {
  background: #[ブランドカラー];
}
.cta-btn:hover {
  background: #[少し暗い色];
}
```

---

## 案件での使い所

- コーポレートサイトのスタッフ紹介セクション
- 個人ポートフォリオの About セクション
- チームメンバー一覧ページ
- LP の制作者・監修者紹介

---

## 実装チェックリスト

```
□ .role / .skill-tag / .cta-btn のカラーをブランドカラーに変更した
□ .avatar のグラデーションをブランドカラーに変更した
□ name / role / bio のテキストを差し替えた
□ skill-tag の数が5個以内になっている
□ bio が3行以内に収まっている
□ SP（640px以下）で表示が崩れていない
□ hover アニメーションが動作している
□ transition に all を使っていない
```

---

## Do / Don't

| ✓ Do | ✗ Don't |
|---|---|
| bio は2〜3行以内に収める | bio に長文を詰め込む |
| スキルタグは5個以内 | タグを10個以上並べる |
| CTA ボタンは1つだけ | ボタンを2つ以上配置する |
| アバターは 88×88px 以上 | 低解像度画像を使う |
| `transition` は個別指定 | `transition: all` を使う |

---

## 更新履歴

| Ver | 日付 | 内容 |
|---|---|---|
| 1.0.0 | 2025-03 | 初版（3バリエーション） |
