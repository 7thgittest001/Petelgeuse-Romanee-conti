# 要件確認質問

以下の質問に回答してください。各質問の `[Answer]:` タグの後に選択肢の文字を記入してください。
どの選択肢にも当てはまらない場合は、最後の選択肢（Other）を選び、説明を追記してください。

---

## Question 1
このアプリのメインターゲットユーザーは誰ですか？

A) 運動不足を自覚しているが実際に走る気力がない20〜30代の社会人
B) ダイエットに興味はあるが継続できない全年齢層
C) ゲーミフィケーション・バーチャル体験が好きなテック好きユーザー
D) 旅行好きだが実際に行けない人（バーチャル旅行としての利用）
E) 上記の複数を含む幅広いユーザー層
X) Other (please describe after [Answer]: tag below)

[Answer]: A, B, 結果的にDも対象に入る可能性もあるが、いったんA,Bとする。

---

## Question 2
MVP（最小限の実用的な製品）として最も重要なコア機能はどれですか？

A) スタート/ゴール指定 → ストリートビュー画像からの動画生成のみ
B) 動画生成 + 手振りアニメーション合成
C) 動画生成 + 手振りアニメーション + マップ上のコース表示
D) 動画生成 + 手振りアニメーション + マップ表示 + 走行記録保存
X) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 3
プラットフォームは何を想定していますか？

A) Webアプリケーション（ブラウザベース）
B) モバイルアプリ（iOS/Android）
C) まずWebアプリ、将来的にモバイル対応
D) まずモバイルアプリ、将来的にWeb対応
X) Other (please describe after [Answer]: tag below)

[Answer]: C

---

## Question 4
ストリートビューの画像取得について、どのAPIの利用を想定していますか？

A) Google Maps Platform（Street View Static API / Street View API）
B) Mapbox（Street-level imagery）
C) 特定のAPIは未定、コスト・機能で最適なものを選びたい
D) 独自の画像ソースも検討したい（ユーザー投稿写真など）
X) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 5
「人をダメにする仕掛け」として、以下のどのアプローチを重視しますか？（複数選択可：例 "A, C"）

A) 習慣化の仕掛け（毎日のストリーク、連続記録、通知リマインダー）
B) ゲーミフィケーション（レベルアップ、バッジ、ランキング、チャレンジ）
C) ソーシャル要素（友人との共有、コース共有、「一緒に走った」感覚）
D) コレクション欲求（コース制覇率、地図の塗りつぶし、未踏破ルート表示）
E) 罪悪感解消の演出（走行距離カウンター、消費カロリー表示、達成感演出）
X) Other (please describe after [Answer]: tag below)

[Answer]: A, B, C, D, E すべてだが、MVPで基礎的機能(動画作成)に寄与しそうなE

---

## Question 6
ユーザー認証・アカウント機能は必要ですか？

A) はい、ユーザー登録・ログイン必須（走行記録の保存のため）
B) はい、ただしソーシャルログイン（Google/Apple）のみで簡易に
C) MVPではアカウント不要（ローカル保存のみ）、将来的に追加
D) アカウント任意（未登録でも使えるが、登録すると記録が保存される）
X) Other (please describe after [Answer]: tag below)

[Answer]: x : 迷っています。提案が欲しい。MVPの実装のしやすさを考えるとアカウント不要ではあるが、MVPの機能によってはユーザーを識別した方が良い可能性もありますね。

---

## Question 7
動画生成の処理はどこで行う想定ですか？

A) サーバーサイド（バックエンドで動画を生成してユーザーに配信）
B) クライアントサイド（ブラウザ/端末上でリアルタイムに画像を切り替え表示）
C) ハイブリッド（画像取得はサーバー、表示・アニメーションはクライアント）
D) 未定、技術的に最適な方法を提案してほしい
X) Other (please describe after [Answer]: tag below)

[Answer]: D

---

## Question 8
収益モデルについてどう考えていますか？

A) 無料アプリ（広告なし）— 個人プロジェクト/ポートフォリオ目的
B) フリーミアム（基本無料 + プレミアム機能は有料）
C) 広告モデル（無料で使えるが広告表示あり）
D) 収益化は現時点では考えていない（まずMVPの実現を優先）
X) Other (please describe after [Answer]: tag below)

[Answer]: まずはA, 後の開発でB, C(後々には動画の隅に広告を入れてもいい)も狙えるといい

---

## Question 9
競合として認識しているサービスはありますか？また、差別化のポイントとして最も重視したいのは？

A) 競合は把握していない。差別化ポイントも未定で提案してほしい
B) Zwift/Pelotonなどのバーチャルフィットネスが競合。差別化は「実際に走らなくていい」点
C) Google Earth Studio/ストリートビュー動画系ツールが競合。差別化は「ランニング体験としての演出」
D) 特に競合は意識せず、ユニークなコンセプト（人をダメにする）自体が差別化
X) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 10
開発チーム構成と技術スキルについて教えてください。

A) 個人開発（フルスタック対応可能）
B) 個人開発（フロントエンド中心、バックエンドは学習中）
C) 小規模チーム（2〜3名）
D) 技術選定も含めて提案してほしい
X) Other (please describe after [Answer]: tag below)

[Answer]: C

---

## Question 11
「手を振っている様子」の動画合成について、どのレベルを想定していますか？

A) シンプルなアニメーション画像（手のイラスト/GIF）を画面端にオーバーレイ
B) ユーザーが事前に撮影した手振り動画を合成
C) AIで生成したリアルな手振り映像を合成
D) MVPでは省略し、将来的に実装
X) Other (please describe after [Answer]: tag below)

[Answer]: D

---

## Question 12
データの保存・管理について、どの程度の規模を想定していますか？

A) 小規模（個人利用〜数十人程度）
B) 中規模（数百〜数千人のユーザー）
C) 大規模（数万人以上を見据えたスケーラブル設計）
D) まずは小規模で開始し、成長に応じてスケールしたい
X) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 13: Security Extensions（セキュリティ拡張）
このプロジェクトにセキュリティ拡張ルールを適用しますか？

A) はい — すべてのセキュリティルールをブロッキング制約として適用する（本番グレードのアプリケーション向け推奨）
B) いいえ — セキュリティルールをスキップする（PoC、プロトタイプ、実験的プロジェクト向け）
X) Other (please describe after [Answer]: tag below)

[Answer]: B

---

## Question 14: Property-Based Testing（プロパティベーステスト拡張）
このプロジェクトにプロパティベーステスト（PBT）ルールを適用しますか？

A) はい — すべてのPBTルールをブロッキング制約として適用する（ビジネスロジック、データ変換、シリアライゼーション、ステートフルコンポーネントを含むプロジェクト向け推奨）
B) 部分的に — 純粋関数とシリアライゼーションのラウンドトリップにのみPBTルールを適用する
C) いいえ — PBTルールをスキップする（シンプルなCRUDアプリ、UIのみのプロジェクト、薄い統合レイヤー向け）
X) Other (please describe after [Answer]: tag below)

[Answer]: B

---

回答が完了しましたら、お知らせください。
