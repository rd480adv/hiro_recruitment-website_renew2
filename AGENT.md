# AGENTS.md - 求人サイトプロトタイプ作成ガイド

このプロジェクトは、ヒロ・スタッフエージェンシー様向けの新求人サイト(西院エリア特化)の**顧客確認用プロトタイプ(紙芝居)**を作成するものです。

## プロジェクト概要

- **目的**: Phase 0における要件確認のための画面遷移プロトタイプ作成
- **成果物**: 相互にリンクされた複数のHTMLページ(紙芝居形式)
- **技術スタック**: HTML + CSS + 最小限のJavaScript(ライブラリ不要)
- **デザイン**: スマホファーストのレスポンシブデザイン
- **期間**: Phase 0(約1ヶ月)内での段階的な更新

## プロトタイプの性質(重要)

### これは「動くプロトタイプ」であり「完成品」ではない

- **目的**: 顧客との要件確認・合意形成
- **精度**: ビジュアルと遷移の確認が主目的。完全な機能実装は不要
- **スピード優先**: 完璧さよりも素早いフィードバックループを重視
- **反復前提**: 顧客フィードバックに基づき何度も修正する

### 実装してはいけないもの

- バックエンドAPIとの実際の通信
- データベース接続
- 実際の認証・認可機能
- 本番環境で必要となるセキュリティ対策の完全実装
- localStorage/sessionStorageの使用(ブラウザストレージAPI)

### 実装すべきもの

- ダミーデータを使った画面表示
- ページ間の遷移(リンク)
- 基本的なJavaScriptによるインタラクション(ボタンクリック、フォーム入力のモック等)
- レスポンシブデザインの確認ができるレイアウト
- 実際のUIコンポーネントの見た目と配置

## 技術仕様

### HTML構造

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[ページタイトル] - SaiinWork</title>
    <style>
        /* インラインCSSを使用 */
    </style>
</head>
<body>
    <!-- コンテンツ -->
    <script>
        /* 最小限のJavaScript */
    </script>
</body>
</html>
```

### ファイル命名規則

- トップページ: `index.html` または `demo_top.html`
- 求人詳細: `demo_job_detail.html`
- 応募フロー: `demo_application_flow.html`
- その他: `demo_[機能名].html`

全てのファイルは同一ディレクトリに配置し、相対パスでリンク。

### CSS設計原則

#### カラーパレット(CSS変数で定義)

```css
:root {
    --primary-color: #FF6B35;      /* メインオレンジ */
    --primary-light: #FF8A65;
    --primary-dark: #E64A19;
    --secondary-color: #FFF3E0;
    --accent-color: #FF9800;
    --accent-warm: #FFF8F3;
    --text-primary: #2C2C2C;
    --text-secondary: #666666;
    --background: #FFFFFF;
    --light-gray: #F5F5F5;
    --success: #4CAF50;
    --error: #F44336;
}
```

#### レスポンシブブレークポイント

```css
/* スマホファースト */
.container {
    max-width: 390px;  /* モバイル */
}

@media (min-width: 768px) {
    .container {
        max-width: 800px;  /* タブレット */
    }
}

@media (min-width: 1024px) {
    .container {
        max-width: 1200px;  /* デスクトップ */
    }
}
```

#### デザイントークン

- **フォント**: システムフォント(`-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto'`)
- **角丸**: `border-radius: 12px` (カード), `border-radius: 8px` (小要素)
- **影**: `box-shadow: 0 2px 12px rgba(0,0,0,0.06)` (カード)
- **余白**: 16px, 24px, 32px の倍数を基本とする

### JavaScript実装ルール

#### 使用可能なJavaScript

- イベントリスナー(`addEventListener`)
- DOM操作(`querySelector`, `getElementById`)
- クラスの追加/削除(`classList`)
- 簡単なアニメーション(CSS transitionと組み合わせ)
- `alert()`, `confirm()` によるモック確認

#### 禁止事項

- **localStorage/sessionStorage の使用**
- 外部ライブラリの読み込み(CDN含む、ただしTailwind CDNは例外的に許可される場合がある)
- 複雑なステート管理
- fetch/XMLHttpRequest によるAPI呼び出し

#### 推奨パターン

```javascript
// ボタンクリック時のモック動作
document.querySelector('.btn-apply').addEventListener('click', function() {
    this.style.background = 'var(--success)';
    this.textContent = '応募完了';
    alert('応募が完了しました!');
});

// タブ切り替え
document.querySelectorAll('.nav-item').forEach(item => {
    item.addEventListener('click', function() {
        // activeクラスの付け替え
        document.querySelectorAll('.nav-item').forEach(i => 
            i.classList.remove('active')
        );
        this.classList.add('active');
    });
});
```

## 画面構成と遷移フロー

### 必須ページ

1. **トップページ** (`index.html`)
   - ヘッダー(ロゴ、ナビゲーション)
   - メインビジュアル(キャッチコピー)
   - 検索セクション
   - おすすめ求人カード一覧
   - 特徴セクション
   - フッター

2. **求人詳細ページ** (`demo_job_detail.html`)
   - 求人ヘッダー情報
   - コミュニティセクション(一緒に働く仲間)
   - 求人詳細情報
   - 職場情報
   - 評価・レビュー
   - 固定ボトムバー(お気に入り・応募ボタン)

3. **応募フローページ** (`demo_application_flow.html`)
   - プログレスバー(4ステップ)
   - Step 1: 求人内容確認
   - Step 2: プロフィール確認・編集
   - Step 3: 最終確認
   - Step 4: 応募完了画面

### ページ間リンク設定

```html
<!-- トップページ → 求人詳細 -->
<a href="demo_job_detail.html">
    <button class="btn-secondary">詳細</button>
</a>

<!-- 求人詳細 → 応募フロー -->
<a href="demo_application_flow.html">
    <button class="btn-apply">今すぐ応募する</button>
</a>

<!-- 戻るボタン -->
<button class="back-btn" onclick="history.back()">←</button>
```

## コーディング規約

### 命名規則

- **CSS クラス名**: ケバブケース(`job-card`, `btn-primary`)
- **JavaScript 変数**: キャメルケース(`currentStep`, `isBookmarked`)
- **ID**: キャメルケース(`applyBtn`, `stepContent1`)

### コード構造

#### HTML

```html
<!-- セマンティックHTMLを使用 -->
<header class="header">
  <nav class="header-nav">...</nav>
</header>

<main class="main-content">
  <section class="hero">...</section>
  <section class="jobs-section">...</section>
</main>

<footer class="footer">...</footer>
```

#### CSS

```css
/* コンポーネント単位でグルーピング */

/* ========== ヘッダー ========== */
.header { }
.header-top { }
.logo { }

/* ========== ジョブカード ========== */
.job-card { }
.job-title { }
.job-wage { }
```

#### JavaScript

```javascript
// コメントで処理の意図を明示
// DOM要素の取得
const applyBtn = document.getElementById('applyBtn');

// イベントリスナー登録
applyBtn.addEventListener('click', function() {
    // 応募処理のモック
    showNotification('応募が完了しました');
});

// ユーティリティ関数
function showNotification(message) {
    // 通知表示処理
}
```

### コメント規約

- 各セクションの開始時にコメントを入れる
- 複雑な処理には必ず説明コメントを付ける
- 日本語でコメントを記述(顧客確認時にわかりやすい)

## ダミーデータの作り方

### 求人データ

```javascript
// サンプル求人データ
const sampleJobs = [
    {
        id: 1,
        title: 'カフェスタッフ',
        company: 'コメダ珈琲店 西院店',
        wage: '¥1,100',
        location: '徒歩2分',
        time: '14:00-18:00',
        featured: true,
        communityCount: 3,
        rating: 4.8,
        reviewCount: 23
    },
    // ... 他の求人
];
```

### ユーザーデータ

```javascript
// ログイン済みユーザー(モック)
const currentUser = {
    name: '山田 太郎',
    phone: '090-1234-5678',
    email: 'yamada@example.com',
    age: 22
};
```

## 顧客フィードバック対応フロー

### フィードバックの受け取り方

1. 顧客からの指摘を箇条書きでリスト化
2. 各項目に優先度を付与(高・中・低)
3. 技術的実現可能性を確認
4. 修正範囲を見積もる

### 修正時の原則

- **小さく素早く**: 大規模な作り直しより段階的な改善
- **1ページずつ**: 全ページ同時修正は避ける
- **コミット単位**: 1つの変更は1つのコミットに
- **確認依頼**: 修正後は速やかに顧客へ確認依頼

### よくある修正パターン

#### カラー変更要望

```css
/* Before */
--primary-color: #FF6B35;

/* After */
--primary-color: #FF9800;  /* より明るいオレンジに変更 */
```

#### レイアウト調整

```css
/* Before */
.search-section {
    margin: -24px 20px 32px;
}

/* After */
.search-section {
    margin: -16px 20px 24px;  /* 余白を縮小 */
}
```

#### テキスト修正

```html
<!-- Before -->
<h1 class="hero-title">西院で働く、仲間とつながる</h1>

<!-- After -->
<h1 class="hero-title">西院で働く、地域とつながる</h1>
```

## テスト・確認項目

### ブラウザ動作確認

- Chrome(最新版)
- Safari(最新版)
- Firefox(最新版)
- モバイルブラウザ(iOS Safari, Chrome)

### 確認項目チェックリスト

- [ ] 全ページが相互にリンクで繋がっている
- [ ] 各ページがスマホサイズで正しく表示される
- [ ] ボタンクリック時の動作が機能する
- [ ] フォーム入力が可能(実際の送信は不要)
- [ ] 画像が正しく表示される(またはプレースホルダーが適切)
- [ ] 日本語の表示に問題がない
- [ ] レスポンシブデザインが機能している(390px, 768px, 1024px)

### 動作確認コマンド

```bash
# ローカルサーバー起動(Pythonの場合)
python -m http.server 8000

# ブラウザで確認
open http://localhost:8000/index.html
```

## 禁止事項まとめ

### 絶対にやってはいけないこと

1. **localStorage/sessionStorage の使用** - Claude artifactsでは動作しない
2. **外部APIへの実際の通信** - プロトタイプには不要
3. **実際のデータベース操作** - ダミーデータで代用
4. **複雑なフレームワークの導入** - React, Vue等は使わない
5. **パスワードの平文保存** - そもそも認証機能は実装しない

### 推奨されないこと

- 過度に複雑なJavaScript
- 100行を超えるJavaScript関数
- インラインスタイルの乱用(CSS変数を活用)
- ID属性の過度な使用(クラス名を優先)

## ファイル構成例

```
project_root/
├── index.html                    # トップページ
├── demo_job_detail.html         # 求人詳細
├── demo_application_flow.html   # 応募フロー
├── demo_scenario_v1.html        # デモシナリオガイド(オプション)
├── AGENTS.md                    # このファイル
└── README.md                    # プロジェクト概要(人間向け)
```

## トラブルシューティング

### よくある問題と解決方法

#### 問題: ページ遷移がうまくいかない

```html
<!-- ❌ 間違い -->
<button onclick="location.href='/demo_job_detail.html'">詳細</button>

<!-- ✅ 正しい -->
<a href="demo_job_detail.html">
    <button class="btn-secondary">詳細</button>
</a>
```

#### 問題: スマホで表示が崩れる

```css
/* viewport設定を確認 */
<meta name="viewport" content="width=device-width, initial-scale=1.0">

/* コンテナ幅の見直し */
.container {
    max-width: 390px;  /* スマホサイズに合わせる */
    padding: 0 20px;   /* 左右の余白を確保 */
}
```

#### 問題: JavaScriptが動かない

```javascript
// DOM読み込み後に実行
window.addEventListener('load', function() {
    // ここに処理を記述
});

// またはDOMContentLoaded
document.addEventListener('DOMContentLoaded', function() {
    // ここに処理を記述
});
```

## ベストプラクティス

### コード品質

- **可読性優先**: 複雑なコードより読みやすいコード
- **コメント充実**: 意図を明確に記述
- **一貫性**: 命名規則とフォーマットを統一
- **DRY原則**: 同じコードの繰り返しは関数化

### パフォーマンス

- **画像最適化**: 不要な大きな画像は避ける(プレースホルダー推奨)
- **CSS in head**: CSSはhead内に記述(インライン)
- **JS at bottom**: JavaScriptはbody終了タグ直前に配置
- **最小限のDOM操作**: 必要最小限に留める

### メンテナビリティ

- **小さなファイル**: 1ファイル500行以内を目安
- **明確なセクション**: コメントで区切る
- **バージョン管理**: 大きな変更前にバックアップ
- **変更履歴**: 重要な修正はコメントに記録

## プロジェクトの進め方

### Phase 0 での位置づけ

1. **Week 1**: 基本3画面(トップ・詳細・応募フロー)の作成
2. **Week 2**: 顧客フィードバック1回目 → 修正
3. **Week 3**: 追加機能(口コミ等)の検討・実装
4. **Week 4**: 最終調整・次フェーズへの引き継ぎ準備

### 次フェーズ(Phase 1)への引き継ぎ

このプロトタイプは Phase 1 での本格開発の**設計図**となります:

- デザイン仕様の確定
- 機能要件の明確化
- ユーザーフローの確認
- 技術的実現可能性の検証

## 参考リソース

### 既存のデモファイル

プロジェクトには以下の参考ファイルが含まれています:

- `sample/demo_top.html` - トップページの実装例
- `sample/demo_top_detail.html` - 求人詳細ページの実装例
- `sample/demo_top_detail_flow.html` - 応募フローの実装例

これらのファイルから、デザインパターン、カラースキーム、JavaScriptの実装パターンを参照してください。

---

## まとめ

このプロジェクトは**顧客確認用プロトタイプ**です。完璧さより**スピードとフィードバック**を優先してください。

- ✅ シンプルで理解しやすいコード
- ✅ 素早い修正対応
- ✅ 視覚的にわかりやすいデモ
- ❌ 過度な技術的完璧さ
- ❌ 本番レベルのセキュリティ
- ❌ 完全な機能実装

**質問があれば、いつでも確認してください。段階的に進めていきましょう。**
