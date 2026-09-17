# アーキテクチャ構成図

フィルム写真愛好家と現像ラボを接続し、AIによる画像一次解析・RAGアシスタントを提供するプラットフォームのクラウド構成図

**クライアント層:**
- フィルム写真愛好家 [エンドユーザー]
- 現像ラボオーナー [ラボ事業者]
- システム管理者 [運営管理者]

**プレゼンテーション層:**
- 愛好家向けモバイルアプリ [Flutter (iOS / Android)]
- ラボ/管理者用Webポータル [ReactJS / Next.js (TypeScript)]

**ゲートウェイ層:**
- CDN / エッジ配信 [Azure Front Door]
- API Gateway [Azure Application Gateway]

**アプリケーション層:**
- バックエンドAPIサーバー [Java Spring Boot]
- リアルタイム通信サーバー [Node.js (Socket.io)]
- AI解析・推薦サービス [Python (OpenCV, PyTorch, LangChain)]

**データ層:**
- プライマリデータベース [Azure Database for PostgreSQL]
- ベクトル検索データベース※要確認 [pgvector / Elasticsearch]
- セッション・キャッシュ※要確認 [Azure Cache for Redis]
- オブジェクトストレージ [Azure Blob Storage (スキャン画像、ネガ保管)]

**外部サービス層:**
- オンライン決済ゲートウェイ [Stripe / VNPay / Momo API]
- オンデマンド配送サービス [Ahamove / Lalamove API]
- 大規模言語モデル [OpenAI GPT-4 API]

**接続:**
- フィルム写真愛好家 → 愛好家向けモバイルアプリ (UI操作)
- 現像ラボオーナー → ラボ/管理者用Webポータル (UI操作)
- システム管理者 → ラボ/管理者用Webポータル (UI操作)
- 愛好家向けモバイルアプリ → CDN / エッジ配信 (HTTPS)
- ラボ/管理者用Webポータル → CDN / エッジ配信 (HTTPS)
- 愛好家向けモバイルアプリ → リアルタイム通信サーバー (WSS)
- CDN / エッジ配信 → API Gateway (HTTPS)
- API Gateway → バックエンドAPIサーバー (HTTP)
- バックエンドAPIサーバー → リアルタイム通信サーバー (gRPC)
- バックエンドAPIサーバー → AI解析・推薦サービス (HTTP)
- バックエンドAPIサーバー → プライマリデータベース (SQL)
- バックエンドAPIサーバー → セッション・キャッシュ※要確認 (Redis Protocol)
- バックエンドAPIサーバー → オブジェクトストレージ (Azure SDK)
- AI解析・推薦サービス → ベクトル検索データベース※要確認 (HTTP/SQL)
- AI解析・推薦サービス → 大規模言語モデル (HTTPS)
- AI解析・推薦サービス → オブジェクトストレージ (Azure SDK)
- バックエンドAPIサーバー → オンライン決済ゲートウェイ (HTTPS)
- バックエンドAPIサーバー → オンデマンド配送サービス (HTTPS)

```mermaid
flowchart TD
    subgraph client["クライアント層"]
        user_amateur["フィルム写真愛好家 (エンドユーザー)"]
        user_lab["現像ラボオーナー (ラボ事業者)"]
        user_admin["システム管理者 (運営管理者)"]
    end
    subgraph presentation["プレゼンテーション層"]
        app_mobile["愛好家向けモバイルアプリ (Flutter (iOS / Android))"]
        app_web_lab["ラボ/管理者用Webポータル (ReactJS / Next.js (TypeScript))"]
    end
    subgraph gateway["ゲートウェイ層"]
        azure_frontdoor["CDN / エッジ配信 (Azure Front Door)"]
        api_gateway["API Gateway (Azure Application Gateway)"]
    end
    subgraph application["アプリケーション層"]
        api_server["バックエンドAPIサーバー (Java Spring Boot)"]
        socket_server["リアルタイム通信サーバー (Node.js (Socket.io))"]
        ai_service["AI解析・推薦サービス (Python (OpenCV, PyTorch, LangChain))"]
    end
    subgraph data["データ層"]
        db_postgres[("プライマリデータベース (Azure Database for PostgreSQL)")]
        db_vector[("ベクトル検索データベース※要確認 (pgvector / Elasticsearch)")]
        cache_redis["セッション・キャッシュ※要確認 (Azure Cache for Redis)"]
        storage_blob["オブジェクトストレージ (Azure Blob Storage (スキャン画像、ネガ保管))"]
    end
    subgraph external["外部サービス層"]
        ext_payment["オンライン決済ゲートウェイ (Stripe / VNPay / Momo API)"]
        ext_delivery["オンデマンド配送サービス (Ahamove / Lalamove API)"]
        ext_llm["大規模言語モデル (OpenAI GPT-4 API)"]
    end
    user_amateur -->|"UI操作"|app_mobile
    user_lab -->|"UI操作"|app_web_lab
    user_admin -->|"UI操作"|app_web_lab
    app_mobile -->|"HTTPS"|azure_frontdoor
    app_web_lab -->|"HTTPS"|azure_frontdoor
    app_mobile -->|"チャット・進捗通知"|socket_server
    azure_frontdoor -->|"HTTPS"|api_gateway
    api_gateway -->|"HTTP"|api_server
    api_server -->|"通知トリガー"|socket_server
    api_server -.->|"非同期AIリクエスト"|ai_service
    api_server -->|"SQL"|db_postgres
    api_server -->|"Redis Protocol"|cache_redis
    api_server -->|"Azure SDK"|storage_blob
    ai_service -->|"RAGセマンティック検索"|db_vector
    ai_service -->|"技術相談生成"|ext_llm
    ai_service -->|"画質評価用画像取得"|storage_blob
    api_server -->|"エスクロー決済・送金"|ext_payment
    api_server -.->|"集荷依頼自動連携"|ext_delivery
```