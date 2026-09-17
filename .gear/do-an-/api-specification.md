# API仕様書

| endpoint | method | description | requestBody | responseBody | auth |
| --- | --- | --- | --- | --- | --- |
| /api/auth/login | POST | メールアドレスとパスワードによる認証を行い、JWTトークンを発行します。MFA（多要素認証）が有効な場合は一時トークンを返します。 | [object Object] |  | [object Object] |
| /api/auth/mfa/verify | POST | ログイン後に表示される認証コード画面において、6桁のワンタイムパスワードを検証し、本認証用のJWTトークンを発行します。 | [object Object] |  | [object Object] |
| /api/auth/password/reset-request | POST | パスワード紛失時、指定されたメールアドレス宛に有効期限付きのパスワードリセット用URLリンクを送信します。 | [object Object] |  | [object Object] |
| /api/auth/logout | POST | ユーザーのセッションを終了し、JWTトークンを無効化します（ブラックリスト登録）。 |  |  | [object Object] |
| /api/auth/token/refresh | POST | リフレッシュトークンを使用して、有効期限切れのアクセストークンを再発行します。 | [object Object] |  | [object Object] |
| /api/users/register | POST | 一般の愛好家ユーザーアカウントを新規仮登録し、本人確認用アクティベーションメールを送信します。 | [object Object] |  | [object Object] |
| /api/users/activate | POST | 送信された確認メール内のトークンを検証し、一般愛好家ユーザーのアカウントを正式に本登録状態（有効化）にします。 | [object Object] |  | [object Object] |
| /api/users/{id}/profile | GET | 一般愛好家ユーザーの個人プロフィール情報、連絡先、お気に入り機材（カメラ・レンズ・フィルム）の一覧を取得します。 |  |  | [object Object] |
| /api/users/{id}/profile | PUT | 一般愛好家ユーザーの個人プロフィール情報、連絡先、お気に入り機材などの編集内容を上書き更新します。 | [object Object] |  | [object Object] |
| /api/labs/{id} | GET | 特定の現像ラボの登録店舗詳細、公式プロフィール、設備状況、住所、営業時間を取得します。 |  |  | [object Object] |
| /api/labs/{id} | PUT | ラボオーナーが、自身の管理ポータルにおいて、店舗名称、公式住所、地図連携用の地理座標を更新します。 | [object Object] |  | [object Object] |
| /api/labs/{id}/services | GET | 該当ラボが提供している現像、スキャン、印刷などの各サービス内容、対応フィルム、現像プロセス、料金、推定納期の一覧を取得します。 |  |  | [object Object] |
| /api/labs/{id}/services | POST | ラボオーナーが、自店舗が新規に対応・販売開始する現像サービスパッケージを新しく追加登録します。 | [object Object] |  | [object Object] |
| /api/labs/{id}/services/{serviceId} | PUT | 既存の現像サービスパッケージの名称、対応プロセス、提供料金、想定納期などを変更・修正します。 | [object Object] |  | [object Object] |
| /api/labs/{id}/services/{serviceId} | DELETE | 非推奨・休止となった対応メニューを削除します。 |  |  | [object Object] |
| /api/labs | GET | 対応フォーマット、現像プロセス、予算上限、最低評価点、および現在地GPS距離から、最適な現像ラボをフィルタ・ソートして一覧取得します。 |  |  | [object Object] |
| /api/recommendations/labs | GET | ログイン中の愛好家ユーザーの過去注文履歴、お気に入り機材、レビューの傾向をAIエンジンが分析し、最適なラボおよび相性の良いおすすめのフィルムを協調フィルタリングを用いて提案します。 |  |  | [object Object] |
| /api/labs/semantic-search | GET | 「レトロな暖かい色合い」「アンダー調の露出に合う現像」などの自然言語によるクエリをAIが理解（ベクトル検索）し、関連度順にラボ及び現像サービスプランを推薦・抽出します。 |  |  | [object Object] |
| /api/orders | POST | 現像、スキャン、印刷などの各種オプションを含む新規注文を作成します。 | [object Object] |  | [object Object] |
| /api/orders/{id} | GET | 指定された注文IDの詳細情報および配送、現像の最新ステータスを取得します。 |  |  | [object Object] |
| /api/orders/{id}/acceptance | PUT | ラボ側ダッシュボードにて新規注文に対して「承認」または「拒否」を設定します。 | [object Object] |  | [object Object] |
| /api/orders/{id}/status | PUT | ラボ側で現像工程ステータス（受領済、現像中、スキャン中、納品準備完了）を更新します。 | [object Object] |  | [object Object] |
| /api/deliveries/estimate | POST | 顧客住所から配送先の現像ラボまでのルート距離から、外部提携配送サービスAPI（Ahamove/Lalamove等）を用いてリアルタイムに配送料を見積もります。 | [object Object] |  | [object Object] |
| /api/deliveries/webhook | POST | 外部配送パートナー（Ahamove等）からのステータス変化コールバックを受け取り、自社データベースと注文進捗を同期します。 | [object Object] |  | [object Object] |
| /api/payments/intent | POST | クレジットカード、地場決済・モバイルマネー等（VNPay, Momo, Stripe等）を介した仮決済用セッションURLを発行します。 | [object Object] |  | [object Object] |
| /api/payments/webhook | POST | 決済ゲートウェイからの支払い完了通知を受け取り、エスクロー（仮受金）を有効化し、手数料を算出・控除記帳します。 | [object Object] |  | [object Object] |
| /api/dashboards/amateur/summary | GET | 進行中の注文数、直近の注文進捗、最近納品されたスキャン画像サムネイルURL3枚、および新着通知件数を高速に集計し返却します。 |  |  | [object Object] |
| /api/notifications/email | POST | 仮会員登録時のアクティベーションや、注文配送ステータスの変化などを捉えて送信キューへの登録を行います。 | [object Object] |  | [object Object] |
| /api/notifications/push | POST | 現像の進捗段階や、配送完了等の変化時、顧客のモバイル（FCM等）へリアルタイムでプッシュ通知を即時送信します。 | [object Object] |  | [object Object] |
| /api/orders/{id}/archives/preview | GET | 仮納品中（検収完了前）の写真データの一覧を、ウォーターマーク（透かし）入りプレビューURLで安全に取得します。 |  |  | [object Object] |
| /api/orders/{id}/archives/inspection | PUT | 仮納品された画像に対し「検収承認（取引完了）」または「不備による再スキャン要求」を確定させます。 | [object Object] |  | [object Object] |
| /api/orders/{id}/archives | POST | 現像ラボ側作業端末から高解像度のスキャン画像ファイルを対象注文へ一括アップロードし、自動マッピング処理を行います。 | [object Object] |  | [object Object] |
| /api/orders/{id}/archives/deliver | PUT | ラボスタッフによる画像アップロード・紐付けが完了した後、顧客へ仮納品されたことを伝える仮納品申請の確定を行います。 |  |  | [object Object] |
| /api/archives/{archive_id}/cv-evaluate | POST | アップロードされた個別画像に対して、コンピュータビジョン（CV）モデルを活用し自動品質判定を実行します。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions | POST | マーケットプレイス機材商品の購入意思を確定し、決済を実行、エスクロー（仮受金）をプラットフォームにプール開始します。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{id}/shipped | PUT | 出品者が配送伝票番号などを登録し、発送完了のアラートを購入者に通知します。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{id}/completed | PUT | 購入者が受取確認を行い、双方の合意に基づいて取引を完了させ、エスクローを解いて出品者売上を確定します。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{id}/messages | POST | フリマ取引内のチャットにおいてメッセージテキストや現物写真URLの送信を行います。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{id}/messages | GET | 特定のフリマ取引に紐づく過去の対話ログ履歴を降順で取得します。 |  |  | [object Object] |
| /api/labs/{id}/kpi | GET | ラボオーナー向けに特定範囲の売上累計、完了注文率、平均処理日数などのデータをチャート表示用に取得します。 |  |  | [object Object] |
| /api/labs/{id}/export | GET | 売上・決済手数料控除などのCSVまたはPDF明細をダウンロードするための公開用一時署名URLを生成します。 |  |  | [object Object] |
| /api/admin/kpi | GET | 管理者ポータル用に注文全体の動向、平均配送、現像リードタイム、決済トラブル状況を月別で集計して取得します。 |  |  | [object Object] |
| /api/admin/moderation/reports | GET | モデレーター・管理者用に一般ユーザーから申請された通報レコードを一覧で取得します。 |  |  | [object Object] |
| /api/admin/moderation/content | PUT | 通報承認や自主規制等に基づき、不適切なフリマ機材出品、コミュニティ投稿を非公開（非表示）化します。 | [object Object] |  | [object Object] |
| /api/admin/moderation/users/{id} | PUT | 違反を繰り返すユーザーに対してのアカウント一時凍結（suspended）、または永久強制利用停止（banned）処置を行います。 | [object Object] |  | [object Object] |
| /api/admin/labs/applications | GET | 新しく参加を希望するフィルムラボ店舗の未承認（pending）申請一覧および添付ライセンスPDF書類の一時署名URLを取得します。 |  |  | [object Object] |
| /api/admin/labs/applications/{id} | PUT | 加盟申請ラボ情報を管理者として「正式承認」または「却下（要理由記入）」を処理します。 | [object Object] |  | [object Object] |
| /api/admin/labs/{id}/commission | PUT | 特定の現像加盟店に対して特別割引（大規模店等向け）など個別の手数料率を管理画面から再定義します。 | [object Object] |  | [object Object] |
| /api/payments/escrow | POST | 現像注文またはマーケットプレイス（フリマ）取引の支払確定時に決済ゲートウェイと連携し、一時プールするエスクローアカウントへ仮受金を作成します。※要確認 | [object Object] |  | [object Object] |
| /api/orders/{id}/tracking | GET | 現像注文の作業進捗（受領、現像中、スキャン中、検収待ち等）と、提携配送パートナーのリアルタイム配送状況およびドライバー現在地を合わせて取得します。※要確認 |  |  | [object Object] |
| /api/orders/{id}/previews | GET | 検収待ち（仮納品完了）ステータスの注文に対し、透かし（ウォーターマーク）入りのプレビュー用デジタル画像URL一覧を取得します。 |  |  | [object Object] |
| /api/orders/{id}/acceptance | POST | 仮納品された画像に対する「検収承認（納品完了・取引完了）」、または不具合を指定しての「再スキャン要求」を送信・記録します。※要確認 | [object Object] |  | [object Object] |
| /api/archives | GET | 検収完了し「active」となったオリジナル写真アーカイブを、カメラ・フィルム銘柄・登録日付・タグなど様々なパラメータで検索・フィルタリングします。 |  |  | [object Object] |
| /api/archives/metadata/{id} | PUT | 写真アーカイブに紐付く、EXIFから自動パースされた機材設定や、手動による撮影カメラ・レンズ・フィルム情報、撮影パラメータ等を編集更新します。※要確認 | [object Object] |  | [object Object] |
| /api/archives/download-zip | POST | ユーザーが選択した複数の写真アーカイブをサーバー側で非同期にZIP圧縮し、ダウンロード可能なバイナリストリームとして提供します。※要確認 | [object Object] |  | [object Object] |
| /api/ai/chat | POST | 専門家ナレッジやQ&Aデータをセマンティック検索(RAG)し、フィルムカメラの撮影方法、おすすめ設定やトラブルシューティングに対する自然言語の回答を生成します。※要確認 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{id}/ship | PUT | 出品者がフリマ商品の発送を完了した際に、配送の追跡伝票番号等を登録してステータスを更新します。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{id}/accept | PUT | 購入者が商品を受け取り、中身を検証したうえで受取確認ボタンをクリックすることで取引が「完了」となります。※要確認 |  |  | [object Object] |
| /api/marketplace/transactions/{id}/chats | POST | フリマ取引当事者間（購入者・出品者）でリアルタイムに連絡を取るためのチャットメッセージまたは画像URLを送信します。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{id}/chats | GET | 取引内における当事者同士のチャットメッセージの過去履歴を取得します。 |  |  | [object Object] |
| /api/labs/orders/{id}/decision | PUT | フィルムラボが到着した新規注文を受理するか、拒否するかの判断を登録します。拒否の際は差し戻し理由が必要となります。 | [object Object] |  | [object Object] |
| /api/labs/orders/{id}/status | PUT | フィルムラボのスタッフが、受注した作業進捗ステージ（received, developing, scanning, ready等）を変更して保存します。 | [object Object] |  | [object Object] |
| /api/labs/orders/{id}/upload-scans | POST | ラボでスキャン完了した複数画像（バイナリ群）をアップロードし、対象の現像注文IDに一括紐付け・仮納品アーカイブを生成します。 | [object Object] |  | [object Object] |
| /api/ai/cv-evaluation | POST | アップロードされたデジタル画像に対してコンピュータビジョン（CV）を適用し、ブレ、ピンボケ、キズを自動検出してスコアリングします。※要確認 | [object Object] |  | [object Object] |
| /api/labs/orders/{id}/cv-alerts | GET | Computer Visionにより画質基準未満と判定され、再スキャンが強く推奨される異常検出画像の一覧アラートを取得します。※要確認 |  |  | [object Object] |
| /api/admin/kpis | GET | プラットフォーム管理者向けに、指定期間における注文件数、平均配送/現像リードタイム、決済トラブル率、および加盟店アクティブ状況の集計データを取得します。※要確認 |  |  | [object Object] |
| /api/admin/moderation/contents/{id} | PUT | 管理者のモデレーション機能により、規約違反と判定された出品商品やレビュー等のコンテンツの公開状態を強制的に「hidden（非表示）」に更新します。 | [object Object] |  | [object Object] |
| /api/admin/moderation/users/{id}/ban | PUT | 規約違反累計の著しいユーザーに対し、アカウントの「一時凍結（suspended）」または「強制退会（banned）」を強制執行し、以降のログインアクセス権限を即時剥奪します。 | [object Object] |  | [object Object] |
| /api/admin/labs/pending | GET | 新規加盟申請中のまま審査中（status = pending）となっているラボの情報および提出された証明書類（営業許可PDF等）のリストを取得します。 |  |  | [object Object] |
| /api/admin/labs/{id}/approval | PUT | 新規申請のあったラボを正式に「承認（approved）」するか「却下（rejected）」し、承認時にはオーナーのユーザーロールを自動で昇格・基本手数料率を割当て適用します。 | [object Object] |  | [object Object] |
| /api/marketplace/products | POST | ユーザーが中古のカメラ、レンズ、フィルム等の機材をフリマプラットフォームへ新規に出品登録します。 | [object Object] |  | [object Object] |
| /api/marketplace/products | GET | 指定された検索キーワード、カテゴリ、コンディション、価格帯等で利用可能なフリマ商品を絞り込み検索します。 |  |  | [object Object] |
| /api/marketplace/products/{product_id} | GET | 特定の中古機材商品の詳細データと出品者のプロフィール要約情報を併せて取得します。 |  |  | [object Object] |
| /api/marketplace/products/{product_id} | PUT | 出品した商品のタイトル、価格、コンディション等の内容を更新します。 | [object Object] |  | [object Object] |
| /api/marketplace/products/{product_id} | DELETE | 不要になった出品商品をプラットフォームから削除（非公開化）します。 |  |  | [object Object] |
| /api/marketplace/transactions/{transaction_id}/shipped | POST | 出品者が購入者に対し商品の発送が完了したことを通知し、追跡リンクや伝票番号を登録します。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{transaction_id}/completed | POST | 購入者が商品を受け取り検品後、受取完了を承認し、エスクロー（仮受金）を解除して出品者の売上高を確定させます。 |  |  | [object Object] |
| /api/marketplace/transactions/{transaction_id}/chats | POST | フリマ取引の当事者間で質問や梱包・配送連絡をするためのメッセージを送信します(RESTフォールバック用)。 | [object Object] |  | [object Object] |
| /api/marketplace/transactions/{transaction_id}/chats | GET | 特定の取引チャット内の過去の送受信メッセージ履歴を一覧取得します。 |  |  | [object Object] |
| /api/labs/applications | POST | 外部の現像ラボ(MiniLab)がプラットフォームに対して出店加盟申請を送付します。 | [object Object] |  | [object Object] |
| /api/labs/{lab_id}/orders | GET | 加盟ラボのダッシュボード上に現在届いている現像・スキャン注文のリストを一覧取得します。 |  |  | [object Object] |
| /api/orders/{order_id}/acceptance | PUT | ラボオーナーが新規に受託した現像注文に対して、承認または拒否の判定を行いステータスを更新します。 | [object Object] |  | [object Object] |
| /api/orders/{order_id}/status | PUT | ラボスタッフが現像進捗度（現像中・スキャン中・納品準備完了など）に応じて現在の進捗ステータスを遷移・更新します。 | [object Object] |  | [object Object] |
| /api/orders/{order_id}/scan-data | POST | 現像・乾燥・スキャンされた写真画像データを一括でドラッグ&ドロップしてサーバーへ格納し、顧客の注文ID情報と自動マッピングします。 | [object Object] |  | [object Object] |
| /api/labs/{labId}/analytics/kpi | GET | ラボオーナーが自身のラボの指定期間における売上総額、完了件数、平均処理日数などの統計データを取得します。※要確認：当日データ以外のキャッシュ化ポリシー |  |  | [object Object] |
| /api/labs/{labId}/analytics/export | GET | 指定年月の取引明細・システム手数料控除額をCSVまたはPDF形式でエクスポートするための一時ダウンロードURLを生成します。 |  |  | [object Object] |
| /api/labs/orders/{orderId}/scans | POST | 現像ラボスタッフがスキャン完了したデジタル画像を一括アップロードし、クラウドストレージにセキュア保存します。 | [object Object] |  | [object Object] |
| /api/labs/orders/{orderId}/scans/map | PATCH | アップロード完了した画像アーカイブ群を正式に対象注文IDに紐付け同期します。 | [object Object] |  | [object Object] |
| /api/labs/orders/{orderId}/scans/submit-delivery | POST | スキャン画像の登録が完了したことをトリガーとし、注文ステータスを仮納品完了へ移行させ、顧客へ検収依頼通知を送出します。 |  |  | [object Object] |
| /api/ai/cv-quality-evaluation | POST | アップロードされた画像に対してコンピュータビジョン(CV)を実行し、ブレや傷ノイズなどを自動検出し一次スコアを返します。※要確認: 外部非同期プロセスの認証方式 | [object Object] |  | [object Object] |
| /api/admin/analytics/kpi | GET | 注文全体動向、平均配送・現像リードタイム、決済トラブル率、加盟店アクティブ率等の主要業務KPI一覧を取得します。当日分以外は深夜バッチ集計テーブルを参照します。 |  |  | [object Object] |
| /api/admin/analytics/kpi/aggregate | POST | 深夜定期実行されるバッチ処理の手動トリガーAPI。前日までの全決済・配送・現像実績を精査・再計算してKPI中間テーブルに集計記録します。 |  |  | [object Object] |
| /api/admin/moderation/contents/{targetId}/hide | PATCH | モデレーター判断に基づき、規約違反が認められた特定の製品出品・レビュー・Q&A等の公開フラグを非表示(hidden)へ変更し、モデレーションログに記録します。 | [object Object] |  | [object Object] |
| /api/admin/moderation/users/{userId}/ban | PATCH | 悪質ユーザーに対するアカウント制限措置を執行します。statusを「suspended」または「banned」にし、現在有効なJWTセッション（※要確認：トークンブラックリスト登録）を強制破棄します。 | [object Object] |  | [object Object] |
| /api/admin/labs/pending-applications | GET | ステータスが 'pending'（審査待ち）の新規加盟申請中のラボ情報一覧を取得します。審査用の一時ドキュメントURLもあわせて返却します。 |  |  | [object Object] |
| /api/admin/labs/{id}/status | PATCH | 管理者がラボ情報を審査し、ステータスを承認（approved）または却下（rejected）に更新します。承認時は対象オーナーの権限をラボオーナーに昇格し、デフォルト手数料率を設定します。却下時は理由の入力を必須とします。 | [object Object] |  | [object Object] |
| /api/admin/labs/{id}/commission-rate | PATCH | 特定の現像ラボに対して、個別システム利用手数料率（特別割引等）を設定・変更します。 | [object Object] |  | [object Object] |