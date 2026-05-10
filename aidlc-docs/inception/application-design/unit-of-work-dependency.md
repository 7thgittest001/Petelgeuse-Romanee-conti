# Unit of Work Dependencies: 走った風になるアプリ

## Dependency Overview

単一ユニット（モノレポ）構成のため、ユニット間の依存関係はありません。
代わりに、内部モジュール間の依存関係を定義します。

---

## Internal Module Dependencies

```
+-------------------+
|    Frontend       |
|    (React)        |
+-------------------+
         |
         | HTTP REST (GET)
         | /api/directions
         | /api/streetview
         v
+-------------------+
|    Backend        |
|    (FastAPI)      |
+-------------------+
         |
         | HTTP REST
         v
+-------------------+
| Google Maps       |
| Platform APIs     |
+-------------------+
```

---

## Dependency Matrix

| From | To | Type | Interface |
|---|---|---|---|
| Frontend | Backend | HTTP REST | GET /api/directions, GET /api/streetview |
| Frontend | Google Maps JS API | JavaScript SDK | Maps JavaScript API (direct) |
| Backend | Google Directions API | HTTP REST | Google API endpoint |
| Backend | Google Street View Static API | HTTP REST | Google API endpoint |

---

## Shared Resources

| Resource | Used By | Management |
|---|---|---|
| Google Maps API Key (Browser) | Frontend | 環境変数（.env） |
| Google Maps API Key (Server) | Backend | Lambda環境変数 / AWS Secrets Manager |
| TypeScript型定義 | Frontend only | src/types/ |
| API契約（エンドポイント仕様） | Frontend ↔ Backend | docs/ に定義 |

---

## Build & Deploy Dependencies

```
Backend (FastAPI)
  ├── Build: pip install -r requirements.txt
  ├── Deploy: SAM deploy → Lambda + API Gateway
  └── 先にデプロイ必須（フロントエンドがAPIエンドポイントURLを必要とする）

Frontend (React)
  ├── Build: npm run build → 静的ファイル生成
  ├── Deploy: S3 sync + CloudFront invalidation
  └── Backend デプロイ後にAPI URLを設定してビルド
```

**デプロイ順序**: Backend → Frontend（Backend のAPI Gateway URLがフロントエンドの環境変数に必要）

---

## Communication Patterns

| Pattern | Usage | Details |
|---|---|---|
| REST API (Sync) | Frontend → Backend | GET リクエスト、JSON レスポンス |
| Client SDK (Sync) | Frontend → Google Maps JS | Maps JavaScript API ライブラリ |
| Proxy (Sync) | Backend → Google APIs | APIキー付きリクエスト転送 |

**非同期通信**: MVPでは不要（将来的に動画生成でWebSocket/ポーリングが必要になる可能性）
