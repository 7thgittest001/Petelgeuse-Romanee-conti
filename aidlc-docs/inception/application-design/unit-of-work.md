# Unit of Work Definition: 走った風になるアプリ

## Unit Structure

**構成**: 1ユニット（モノレポ）
**開発順序**: バックエンド先行

このプロジェクトは単一のモノレポとして管理し、フロントエンドとバックエンドを1つのリポジトリ内に配置します。

---

## Unit: virtual-running-app（単一ユニット）

### 概要
| 項目 | 値 |
|---|---|
| **ユニット名** | virtual-running-app |
| **構成** | モノレポ（Frontend + Backend） |
| **デプロイ先** | Frontend: S3+CloudFront / Backend: Lambda+API Gateway |
| **開発チーム** | 2〜3名 |

### 責務
- バーチャルランニング体験の全機能を提供
- Google Maps Platform APIとの連携
- ユーザーインターフェースの表示・操作

### 内部モジュール構成

```
virtual-running-app/
├── frontend/                    ← React アプリケーション
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── contexts/
│   │   ├── data/
│   │   ├── types/
│   │   ├── lib/
│   │   └── App.tsx
│   ├── public/
│   ├── package.json
│   ├── vite.config.ts
│   └── tsconfig.json
├── backend/                     ← FastAPI アプリケーション
│   ├── app/
│   │   ├── main.py
│   │   ├── routers/
│   │   │   ├── directions.py
│   │   │   └── streetview.py
│   │   └── config.py
│   ├── requirements.txt
│   └── template.yaml           ← SAM テンプレート（Lambda デプロイ用）
├── infrastructure/              ← インフラ定義（将来的に拡張）
│   └── README.md
├── docs/                        ← プロジェクトドキュメント
├── .gitignore
├── README.md
└── Makefile                     ← 共通コマンド（build, deploy, test等）
```

---

## 開発順序

### Phase A: バックエンド（1〜2日）
1. FastAPI プロジェクト初期化
2. `/api/directions` エンドポイント実装
3. `/api/streetview` エンドポイント実装
4. `/api/health` エンドポイント実装
5. ローカルテスト
6. Lambda デプロイ（SAM）

### Phase B: フロントエンド（3〜5日）
1. React + Vite プロジェクト初期化
2. TopPage（地図 + コース選択）
3. RunningPage（Street View表示 + カウンター + ミニマップ）
4. CompletionPage（完走演出）
5. 共犯者トーンメッセージ実装
6. バックエンドAPI接続

### Phase C: 統合・デプロイ（1日）
1. フロントエンド → S3 + CloudFront デプロイ
2. E2Eテスト
3. 動作確認

---

## 開発順序の根拠

| 理由 | 説明 |
|---|---|
| バックエンドが小さい | APIプロキシのみ（数十行）で1〜2日で完成 |
| モックデータ不要 | 先にAPIが動いていれば、フロントエンドは実データで開発できる |
| 早期にAPI動作確認 | Google Maps APIとの接続問題を早期に発見できる |
| フロントエンド開発が快適 | 実際のStreet View画像を見ながらUI調整できる |
