# 画面遷移

プラットフォーム全体の主要な画面遷移図。一般愛好家のラボ現像・デジタルアーカイブ利用・中古機材フリマ・コミュニティへのアクセスから、加盟ラボの受注管理、システム管理者による承認・モデレーション業務に至るまでのナビゲーションパスを定義しています。

**フロー:**
- ログイン画面 → 会員登録・本人確認画面 (新規登録ボタン)
- ログイン画面 → 愛好家ダッシュボード (ログイン（愛好家権限）)
- ログイン画面 → ラボ売上ダッシュボード・受注管理画面 (ログイン（ラボオーナー権限）)
- ログイン画面 → システム管理者ポータル画面 (ログイン（システム管理者権限）)
- 会員登録・本人確認画面 → ログイン画面 (登録完了・アクティベーション)
- 愛好家ダッシュボード → ラボ検索・比較画面 (現像ラボを探す)
- ラボ検索・比較画面 → ラボ詳細・サービス一覧画面 (ラボ詳細を表示)
- ラボ詳細・サービス一覧画面 → 注文作成・配送選択画面 (現像・スキャンを注文)
- 注文作成・配送選択画面 → オンライン決済画面 (決済に進む)
- オンライン決済画面 → 注文進捗追跡画面 (決済成功)
- 愛好家ダッシュボード → 注文進捗追跡画面 (注文履歴を確認)
- 注文進捗追跡画面 → 仮納品プレビュー・検収画面 (仮納品データのプレビュー)
- 仮納品プレビュー・検収画面 → デジタルアーカイブ（ギャラリー）画面 (検収承認（納品完了）)
- 愛好家ダッシュボード → デジタルアーカイブ（ギャラリー）画面 (デジタルアーカイブを開く)
- デジタルアーカイブ（ギャラリー）画面 → 写真詳細・EXIFメタデータ編集画面 (写真を選択)
- 愛好家ダッシュボード → AI写真技術相談チャット画面 (AI写真技術相談チャット)
- 愛好家ダッシュボード → 中古機材フリマ検索画面 (フリマを閲覧)
- 中古機材フリマ検索画面 → フリマ商品詳細画面 (商品を選択)
- フリマ商品詳細画面 → オンライン決済画面 (フリマ機材を購入)
- フリマ商品詳細画面 → フリマ取引連絡・チャット画面 (出品者に質問)
- 愛好家ダッシュボード → フリマ機材出品画面 (中古機材を出品)
- 愛好家ダッシュボード → フリマ取引連絡・チャット画面 (フリマ取引履歴)
- フリマ取引連絡・チャット画面 → フリマ相互評価登録画面 (取引完了・評価する)
- 愛好家ダッシュボード → コミュニティQ&A一覧・詳細画面 (コミュニティQ&A)
- 愛好家ダッシュボード → 専門家ナレッジ記事一覧・詳細画面 (技術記事を読む)
- 専門家ナレッジ記事一覧・詳細画面 → 専門家記事執筆・管理画面 (新規執筆申請（専門家のみ）)
- 愛好家ダッシュボード → イベント・ワークショップ一覧画面 (イベントを探す)
- 愛好家ダッシュボード → フィルムラボ加盟申請画面 (ラボとして加盟申請)
- フィルムラボ加盟申請画面 → ログイン画面 (申請書提出完了)
- ラボ売上ダッシュボード・受注管理画面 → スキャンデータ一括アップロード画面 (スキャンデータ一括登録)
- ラボ売上ダッシュボード・受注管理画面 → 愛好家ダッシュボード (愛好家機能を利用)
- システム管理者ポータル画面 → 新規加盟ラボ審査画面 (加盟ラボ審査メニュー)
- システム管理者ポータル画面 → ナレッジ記事審査ワークフロー画面 (記事公開審査メニュー)
- システム管理者ポータル画面 → 違反通報・モデレーション管理画面 (通報・モデレーション管理)

```mermaid
flowchart TD
    login["ログイン画面"]
    register["会員登録・本人確認画面"]
    dashboard_customer["愛好家ダッシュボード"]
    lab_search["ラボ検索・比較画面"]
    lab_detail["ラボ詳細・サービス一覧画面"]
    order_create["注文作成・配送選択画面"]
    payment["オンライン決済画面"]
    order_status["注文進捗追跡画面"]
    inspection["仮納品プレビュー・検収画面"]
    archive["デジタルアーカイブ（ギャラリー）画面"]
    photo_detail["写真詳細・EXIFメタデータ編集画面"]
    ai_chat["AI写真技術相談チャット画面"]
    marketplace_list["中古機材フリマ検索画面"]
    marketplace_detail["フリマ商品詳細画面"]
    marketplace_exhibit["フリマ機材出品画面"]
    marketplace_chat["フリマ取引連絡・チャット画面"]
    marketplace_review["フリマ相互評価登録画面"]
    qa_list["コミュニティQ&A一覧・詳細画面"]
    expert_articles["専門家ナレッジ記事一覧・詳細画面"]
    expert_article_edit["専門家記事執筆・管理画面"]
    event_list["イベント・ワークショップ一覧画面"]
    lab_apply["フィルムラボ加盟申請画面"]
    dashboard_lab["ラボ売上ダッシュボード・受注管理画面"]
    lab_upload["スキャンデータ一括アップロード画面"]
    dashboard_admin["システム管理者ポータル画面"]
    admin_lab_verify["新規加盟ラボ審査画面"]
    admin_article_verify["ナレッジ記事審査ワークフロー画面"]
    admin_moderation["違反通報・モデレーション管理画面"]
    login -->|"新規登録ボタン"|register
    login -->|"ログイン（愛好家権限）"|dashboard_customer
    login -->|"ログイン（ラボオーナー権限）"|dashboard_lab
    login -->|"ログイン（システム管理者権限）"|dashboard_admin
    register -->|"登録完了・アクティベーション"|login
    dashboard_customer -->|"現像ラボを探す"|lab_search
    lab_search -->|"ラボ詳細を表示"|lab_detail
    lab_detail -->|"現像・スキャンを注文"|order_create
    order_create -->|"決済に進む"|payment
    payment -->|"決済成功"|order_status
    dashboard_customer -->|"注文履歴を確認"|order_status
    order_status -->|"仮納品データのプレビュー"|inspection
    inspection -->|"検収承認（納品完了）"|archive
    dashboard_customer -->|"デジタルアーカイブを開く"|archive
    archive -->|"写真を選択"|photo_detail
    dashboard_customer -->|"AI写真技術相談チャット"|ai_chat
    dashboard_customer -->|"フリマを閲覧"|marketplace_list
    marketplace_list -->|"商品を選択"|marketplace_detail
    marketplace_detail -->|"フリマ機材を購入"|payment
    marketplace_detail -->|"出品者に質問"|marketplace_chat
    dashboard_customer -->|"中古機材を出品"|marketplace_exhibit
    dashboard_customer -->|"フリマ取引履歴"|marketplace_chat
    marketplace_chat -->|"取引完了・評価する"|marketplace_review
    dashboard_customer -->|"コミュニティQ&A"|qa_list
    dashboard_customer -->|"技術記事を読む"|expert_articles
    expert_articles -->|"新規執筆申請（専門家のみ）"|expert_article_edit
    dashboard_customer -->|"イベントを探す"|event_list
    dashboard_customer -->|"ラボとして加盟申請"|lab_apply
    lab_apply -->|"申請書提出完了"|login
    dashboard_lab -->|"スキャンデータ一括登録"|lab_upload
    dashboard_lab -->|"愛好家機能を利用"|dashboard_customer
    dashboard_admin -->|"加盟ラボ審査メニュー"|admin_lab_verify
    dashboard_admin -->|"記事公開審査メニュー"|admin_article_verify
    dashboard_admin -->|"通報・モデレーション管理"|admin_moderation
```