# 画面UI定義書

```json
{
  "screens": [
    {
      "screenId": "SCR-001",
      "screenName": "ログイン画面",
      "category": "アカウント管理",
      "targetUser": "すべてのユーザー",
      "overview": "システムにログインするための画面。多要素認証（MFA）が有効な場合は、認証コード入力画面をモーダル表示する。パスワード再設定用のメール送信トリガーも備える。",
      "components": [
        {
          "name": "ログインフォーム",
          "type": "form",
          "description": "メールアドレス、パスワードを入力するフォーム"
        },
        {
          "name": "MFA入力モーダル",
          "type": "modal",
          "description": "多要素認証コードを入力するためのモーダルウィンドウ"
        },
        {
          "name": "ソーシャルログイン連携エリア",
          "type": "button-group",
          "description": "Google, FacebookなどのSNS認証連携用ボタン"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "メールアドレスとパスワードを入力してログインボタンを押下する",
          "systemResponse": "入力情報を検証し、MFAが無効ならダッシュボードへ遷移。有効ならMFA入力モーダルを表示する。"
        },
        {
          "step": 2,
          "action": "MFA有効ユーザーが、認証アプリに届いた6桁のワンタイムパスワードを入力する",
          "systemResponse": "コードを検証し、正しければダッシュボードへ遷移する。"
        }
      ],
      "fields": [
        {
          "name": "メールアドレス",
          "type": "email",
          "required": true,
          "validation": "RFC規格準拠, 必須入力",
          "description": "登録済みのログイン用メールアドレス"
        },
        {
          "name": "パスワード",
          "type": "password",
          "required": true,
          "validation": "半角英数8文字以上, 必須入力",
          "description": "ユーザーのログインパスワード"
        },
        {
          "name": "認証コード",
          "type": "number",
          "required": false,
          "validation": "半角数字6桁",
          "description": "MFA有効時に認証アプリ等から取得するコード"
        }
      ],
      "events": [
        {
          "trigger": "ログインボタンクリック",
          "action": "認証API呼び出し",
          "description": "入力された認証情報をサーバーに送信し、検証結果に基づきログイン処理またはエラーメッセージ表示を行う。"
        },
        {
          "trigger": "パスワードを忘れた場合のリンククリック",
          "action": "パスワードリセット要求",
          "description": "パスワードリセット用トークンを記載したメールの配信依頼画面へ誘導する。"
        }
      ],
      "transitions": [
        {
          "action": "新規登録ボタンクリック",
          "destination": "SCR-002",
          "condition": "アカウントがない場合"
        },
        {
          "action": "ログイン完了（愛好家）",
          "destination": "SCR-003",
          "condition": "ロールが愛好家の場合"
        },
        {
          "action": "ログイン完了（ラボ）",
          "destination": "SCR-023",
          "condition": "ロールがラボオーナーの場合"
        },
        {
          "action": "ログイン完了（管理者）",
          "destination": "SCR-025",
          "condition": "ロールが管理者の場合"
        }
      ],
      "mockHtml": "<!DOCTYPE html>\n<html lang=\"ja\">\n<head>\n  <meta charset=\"UTF-8\">\n  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n  <title>ログイン | SCR-001</title>\n  <style>\n    :root {\n      --primary: #2563eb;\n      --primary-hover: #1d4ed8;\n      --primary-light: #eff6ff;\n      --text-dark: #1e293b;\n      --text-medium: #475569;\n      --text-light: #94a3b8;\n      --bg-main: #f8fafc;\n      --bg-card: #ffffff;\n      --border: #e2e8f0;\n      --border-focus: #3b82f6;\n      --success: #16a34a;\n      --error: #dc2626;\n      --warning: #d97706;\n      --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.05);\n      --transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);\n    }\n\n    * {\n      box-sizing: border-box;\n      margin: 0;\n      padding: 0;\n      font-family: -apple-system, BlinkMacSystemFont, \"Segoe UI\", Roboto, \"Helvetica Neue\", Arial, \"Noto Sans\", sans-serif;\n    }\n\n    body {\n      background-color: var(--bg-main);\n      color: var(--text-dark);\n      min-height: 100vh;\n      display: flex;\n      flex-direction: column;\n      align-items: center;\n      justify-content: center;\n      padding: 24px;\n    }\n\n    .login-container {\n      width: 100%;\n      max-width: 440px;\n      background: var(--bg-card);\n      border-radius: 16px;\n      box-shadow: var(--shadow);\n      border: 1px solid var(--border);\n      padding: 40px 32px;\n      transition: var(--transition);\n    }\n\n    .logo-area {\n      display: flex;\n      flex-direction: column;\n      align-items: center;\n      margin-bottom: 32px;\n    }\n\n    .logo-icon {\n      width: 48px;\n      height: 48px;\n      background-color: var(--primary-light);\n      color: var(--primary);\n      border-radius: 12px;\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      margin-bottom: 16px;\n    }\n\n    .logo-icon svg {\n      width: 28px;\n      height: 28px;\n    }\n\n    h1 {\n      font-size: 24px;\n      font-weight: 700;\n      color: var(--text-dark);\n      margin-bottom: 8px;\n      text-align: center;\n    }\n\n    .subtitle {\n      font-size: 14px;\n      color: var(--text-medium);\n      text-align: center;\n    }\n\n    .form-group {\n      margin-bottom: 20px;\n      position: relative;\n    }\n\n    .form-label {\n      display: block;\n      font-size: 14px;\n      font-weight: 600;\n      color: var(--text-medium);\n      margin-bottom: 8px;\n    }\n\n    .form-label .required {\n      color: var(--error);\n      margin-left: 4px;\n    }\n\n    .input-wrapper {\n      position: relative;\n      display: flex;\n      align-items: center;\n    }\n\n    .input-icon {\n      position: absolute;\n      left: 14px;\n      color: var(--text-light);\n      width: 20px;\n      height: 20px;\n      display: flex;\n      align-items: center;\n      justify-content: center;\n    }\n\n    .input-icon svg {\n      width: 100%;\n      height: 100%;\n    }\n\n    .form-input {\n      width: 100%;\n      padding: 12px 14px 12px 44px;\n      border: 1px solid var(--border);\n      border-radius: 8px;\n      font-size: 15px;\n      color: var(--text-dark);\n      background-color: #fff;\n      transition: var(--transition);\n      outline: none;\n    }\n\n    .form-input:focus {\n      border-color: var(--border-focus);\n      box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.1);\n    }\n\n    .password-toggle {\n      position: absolute;\n      right: 14px;\n      background: none;\n      border: none;\n      color: var(--text-light);\n      cursor: pointer;\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      padding: 4px;\n    }\n\n    .password-toggle:hover {\n      color: var(--text-medium);\n    }\n\n    .password-toggle svg {\n      width: 20px;\n      height: 20px;\n    }\n\n    .form-options {\n      display: flex;\n      align-items: center;\n      justify-content: space-between;\n      margin-bottom: 24px;\n      font-size: 14px;\n    }\n\n    .remember-me {\n      display: flex;\n      align-items: center;\n      gap: 8px;\n      color: var(--text-medium);\n      cursor: pointer;\n    }\n\n    .remember-me input[type=\"checkbox\"] {\n      width: 16px;\n      height: 16px;\n      border-radius: 4px;\n      border: 1px solid var(--border);\n      accent-color: var(--primary);\n      cursor: pointer;\n    }\n\n    .forgot-password {\n      color: var(--primary);\n      text-decoration: none;\n      font-weight: 500;\n    }\n\n    .forgot-password:hover {\n      text-decoration: underline;\n    }\n\n    .btn {\n      width: 100%;\n      padding: 12px 24px;\n      border: none;\n      border-radius: 8px;\n      font-size: 16px;\n      font-weight: 600;\n      cursor: pointer;\n      display: inline-flex;\n      align-items: center;\n      justify-content: center;\n      gap: 8px;\n      transition: var(--transition);\n    }\n\n    .btn-primary {\n      background-color: var(--primary);\n      color: #fff;\n    }\n\n    .btn-primary:hover {\n      background-color: var(--primary-hover);\n    }\n\n    .btn-primary:active {\n      transform: scale(0.98);\n    }\n\n    .divider {\n      display: flex;\n      align-items: center;\n      text-align: center;\n      margin: 24px 0;\n      color: var(--text-light);\n      font-size: 13px;\n    }\n\n    .divider::before, .divider::after {\n      content: '';\n      flex: 1;\n      border-bottom: 1px solid var(--border);\n    }\n\n    .divider:not(:empty)::before {\n      margin-right: .5em;\n    }\n\n    .divider:not(:empty)::after {\n      margin-left: .5em;\n    }\n\n    .social-group {\n      display: grid;\n      grid-template-columns: repeat(2, 1fr);\n      gap: 12px;\n      margin-bottom: 24px;\n    }\n\n    .btn-social {\n      background-color: #fff;\n      border: 1px solid var(--border);\n      color: var(--text-medium);\n      font-size: 14px;\n      font-weight: 500;\n      padding: 10px 16px;\n    }\n\n    .btn-social:hover {\n      background-color: var(--bg-main);\n      border-color: var(--text-light);\n    }\n\n    .btn-social svg {\n      width: 18px;\n      height: 18px;\n    }\n\n    .signup-prompt {\n      text-align: center;\n      font-size: 14px;\n      color: var(--text-medium);\n    }\n\n    .signup-link {\n      color: var(--primary);\n      text-decoration: none;\n      font-weight: 600;\n    }\n\n    .signup-link:hover {\n      text-decoration: underline;\n    }\n\n    /* MFA Modal Styles */\n    .modal-overlay {\n      position: fixed;\n      top: 0;\n      left: 0;\n      right: 0;\n      bottom: 0;\n      background-color: rgba(15, 23, 42, 0.6);\n      backdrop-filter: blur(4px);\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      z-index: 1000;\n      opacity: 0;\n      pointer-events: none;\n      transition: opacity 0.3s ease;\n    }\n\n    .modal-overlay.active {\n      opacity: 1;\n      pointer-events: auto;\n    }\n\n    .modal-content {\n      background: var(--bg-card);\n      border-radius: 16px;\n      box-shadow: var(--shadow);\n      border: 1px solid var(--border);\n      width: 100%;\n      max-width: 400px;\n      padding: 32px;\n      transform: translateY(20px);\n      transition: transform 0.3s ease;\n    }\n\n    .modal-overlay.active .modal-content {\n      transform: translateY(0);\n    }\n\n    .modal-header {\n      text-align: center;\n      margin-bottom: 24px;\n    }\n\n    .modal-icon {\n      width: 48px;\n      height: 48px;\n      background-color: #fef3c7;\n      color: var(--warning);\n      border-radius: 50%;\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      margin: 0 auto 16px auto;\n    }\n\n    .modal-icon svg {\n      width: 24px;\n      height: 24px;\n    }\n\n    .modal-title {\n      font-size: 20px;\n      font-weight: 700;\n      color: var(--text-dark);\n      margin-bottom: 8px;\n    }\n\n    .modal-desc {\n      font-size: 14px;\n      color: var(--text-medium);\n      line-height: 1.5;\n    }\n\n    .mfa-code-inputs {\n      display: flex;\n      justify-content: space-between;\n      gap: 8px;\n      margin: 24px 0;\n    }\n\n    .mfa-input {\n      width: 48px;\n      height: 56px;\n      border: 2px solid var(--border);\n      border-radius: 8px;\n      font-size: 24px;\n      font-weight: 700;\n      text-align: center;\n      color: var(--text-dark);\n      outline: none;\n      transition: var(--transition);\n    }\n\n    .mfa-input:focus {\n      border-color: var(--border-focus);\n      box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.1);\n    }\n\n    .modal-actions {\n      display: flex;\n      flex-direction: column;\n      gap: 12px;\n    }\n\n    .btn-secondary {\n      background-color: var(--bg-main);\n      color: var(--text-medium);\n      border: 1px solid var(--border);\n    }\n\n    .btn-secondary:hover {\n      background-color: var(--border);\n    }\n\n    /* Demo Controls */\n    .demo-controls {\n      margin-top: 32px;\n      padding: 16px;\n      background-color: var(--bg-card);\n      border: 1px dashed var(--border);\n      border-radius: 12px;\n      max-width: 440px;\n      width: 100%;\n    }\n\n    .demo-title {\n      font-size: 12px;\n      font-weight: 700;\n      text-transform: uppercase;\n      color: var(--text-light);\n      margin-bottom: 12px;\n      letter-spacing: 0.05em;\n    }\n\n    .demo-buttons {\n      display: flex;\n      flex-wrap: wrap;\n      gap: 8px;\n    }\n\n    .demo-btn {\n      padding: 6px 12px;\n      font-size: 12px;\n      border: 1px solid var(--border);\n      background-color: #fff;\n      border-radius: 6px;\n      cursor: pointer;\n      color: var(--text-medium);\n      transition: var(--transition);\n    }\n\n    .demo-btn:hover {\n      background-color: var(--primary-light);\n      color: var(--primary);\n      border-color: var(--primary);\n    }\n  </style>\n</head>\n<body>\n\n  <div class=\"login-container\">\n    <div class=\"logo-area\">\n      <div class=\"logo-icon\">\n        <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2.5\">\n          <path d=\"M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5\"/>\n        </svg>\n      </div>\n      <h1>おかえりなさい</h1>\n      <p class=\"subtitle\">アカウント情報を入力してログインしてください</p>\n    </div>\n\n    <form id=\"loginForm\" onsubmit=\"handleLogin(event)\">\n      <div class=\"form-group\">\n        <label class=\"form-label\" for=\"email\">メールアドレス<span class=\"required\">*</span></label>\n        <div class=\"input-wrapper\">\n          <span class=\"input-icon\">\n            <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\"><path d=\"M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z\"/><polyline points=\"22,6 12,13 2,6\"/></svg>\n          </span>\n          <input \n            type=\"email\" \n            id=\"email\" \n            class=\"form-input\" \n            placeholder=\"example@domain.com\" \n            value=\"sato.hanako@example.co.jp\"\n            required\n          >\n        </div>\n      </div>\n\n      <div class=\"form-group\">\n        <label class=\"form-label\" for=\"password\">パスワード<span class=\"required\">*</span></label>\n        <div class=\"input-wrapper\">\n          <span class=\"input-icon\">\n            <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\"><rect x=\"3\" y=\"11\" width=\"18\" height=\"11\" rx=\"2\" ry=\"2\"/><path d=\"M7 11V7a5 5 0 0 1 10 0v4\"/></svg>\n          </span>\n          <input \n            type=\"password\" \n            id=\"password\" \n            class=\"form-input\" \n            placeholder=\"••••••••\" \n            value=\"SecurePass1234!\"\n            required\n          >\n          <button type=\"button\" class=\"password-toggle\" onclick=\"togglePasswordVisibility()\" aria-label=\"パスワード表示切替\">\n            <svg id=\"eyeIcon\" viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\">\n              <path d=\"M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z\"/>\n              <circle cx=\"12\" cy=\"12\" r=\"3\"/>\n            </svg>\n          </button>\n        </div>\n      </div>\n\n      <div class=\"form-options\">\n        <label class=\"remember-me\">\n          <input type=\"checkbox\" id=\"remember\" checked>\n          <span>ログイン状態を保持する</span>\n        </label>\n        <a href=\"#\" class=\"forgot-password\" onclick=\"triggerPasswordReset()\">パスワードを忘れた場合</a>\n      </div>\n\n      <button type=\"submit\" class=\"btn btn-primary\">\n        ログイン\n        <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\" style=\"width:18px; height:18px;\"><path d=\"M5 12h14M12 5l7 7-7 7\"/></svg>\n      </button>\n    </form>\n\n    <div class=\"divider\">または以下でログイン</div>\n\n    <div class=\"social-group\">\n      <button class=\"btn btn-social\">\n        <svg viewBox=\"0 0 24 24\" fill=\"currentColor\">\n          <path d=\"M12.24 10.285V13.4h6.887c-.275 1.565-1.88 4.604-6.887 4.604-4.33 0-7.866-3.577-7.866-8s3.536-8 7.866-8c2.46 0 4.105 1.025 5.047 1.926l2.427-2.334C17.955 2.192 15.34 1 12.24 1 5.48 1 0 6.48 0 13s5.48 12 12.24 12c7.06 0 11.75-4.97 11.75-11.95 0-.8-.08-1.41-.18-1.765H12.24z\"/>\n        </svg>\n        Google\n      </button>\n      <button class=\"btn btn-social\">\n        <svg viewBox=\"0 0 24 24\" fill=\"currentColor\">\n          <path d=\"M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z\"/>\n        </svg>\n        Facebook\n      </button>\n    </div>\n\n    <div class=\"signup-prompt\">\n      アカウントをお持ちでないですか？ \n      <a href=\"#\" class=\"signup-link\" onclick=\"navigate('SCR-002', '新規登録画面へ遷移します')\">新規登録 (SCR-002)</a>\n    </div>\n  </div>\n\n  <!-- MFA Modal -->\n  <div class=\"modal-overlay\" id=\"mfaModal\">\n    <div class=\"modal-content\">\n      <div class=\"modal-header\">\n        <div class=\"modal-icon\">\n          <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\">\n            <rect x=\"5\" y=\"2\" width=\"14\" height=\"20\" rx=\"2\" ry=\"2\"/>\n            <line x1=\"12\" y1=\"18\" x2=\"12.01\" y2=\"18\"/>\n          </svg>\n        </div>\n        <h2 class=\"modal-title\">多要素認証 (MFA)</h2>\n        <p class=\"modal-desc\">登録済みの認証アプリに表示されている6桁の確認コードを入力してください。</p>\n      </div>\n\n      <div class=\"mfa-code-inputs\">\n        <input type=\"text\" class=\"mfa-input\" maxlength=\"1\" value=\"5\" onkeyup=\"moveFocus(this, 1)\" id=\"mfa1\">\n        <input type=\"text\" class=\"mfa-input\" maxlength=\"1\" value=\"2\" onkeyup=\"moveFocus(this, 2)\" id=\"mfa2\">\n        <input type=\"text\" class=\"mfa-input\" maxlength=\"1\" value=\"0\" onkeyup=\"moveFocus(this, 3)\" id=\"mfa3\">\n        <input type=\"text\" class=\"mfa-input\" maxlength=\"1\" value=\"9\" onkeyup=\"moveFocus(this, 4)\" id=\"mfa4\">\n        <input type=\"text\" class=\"mfa-input\" maxlength=\"1\" value=\"8\" onkeyup=\"moveFocus(this, 5)\" id=\"mfa5\">\n        <input type=\"text\" class=\"mfa-input\" maxlength=\"1\" value=\"4\" onkeyup=\"moveFocus(this, 6)\" id=\"mfa6\">\n      </div>\n\n      <div class=\"modal-actions\">\n        <button type=\"button\" class=\"btn btn-primary\" onclick=\"verifyMFACode()\">\n          認証してログイン\n        </button>\n        <button type=\"button\" class=\"btn btn-secondary\" onclick=\"closeMfaModal()\">キャンセル</button>\n      </div>\n    </div>\n  </div>\n\n  <!-- Demo / Test Controls -->\n  <div class=\"demo-controls\">\n    <div class=\"demo-title\">開発・テスト用シミュレーター</div>\n    <p style=\"font-size: 11px; color: var(--text-medium); margin-bottom: 8px;\">\n      ダッシュボード遷移時のターゲットロールを選択してからログインを実行、またはMFAを起動してください：\n    </p>\n    <div style=\"margin-bottom: 12px;\">\n      <label style=\"font-size: 12px; font-weight: 600; color: var(--text-medium);\">\n        ターゲットロール:\n        <select id=\"roleSelector\" style=\"margin-left: 8px; padding: 4px; border-radius: 4px; border:1px solid var(--border); outline: none;\">\n          <option value=\"SCR-003\">愛好家 (SCR-003)</option>\n          <option value=\"SCR-023\">ラボオーナー (SCR-023)</option>\n          <option value=\"SCR-025\">管理者 (SCR-025)</option>\n        </select>\n      </label>\n    </div>\n    <div class=\"demo-buttons\">\n      <button class=\"demo-btn\" onclick=\"openMfaModal()\">MFAモーダルを直接開く</button>\n      <button class=\"demo-btn\" onclick=\"fillDemoUser('yamada.taro@example.com', 'TaroPass5678!')\">愛好家データ入力</button>\n      <button class=\"demo-btn\" onclick=\"fillDemoUser('owner.lab@example.co.jp', 'LabOwner999#')\">ラボオーナーデータ入力</button>\n    </div>\n  </div>\n\n  <script>\n    // Toggle Password Visibility\n    function togglePasswordVisibility() {\n      const passwordInput = document.getElementById('password');\n      const eyeIcon = document.getElementById('eyeIcon');\n      \n      if (passwordInput.type === 'password') {\n        passwordInput.type = 'text';\n        eyeIcon.innerHTML = `\n          <path d=\"M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24\"/>\n          <line x1=\"1\" y1=\"1\" x2=\"23\" y2=\"23\"/>\n        `;\n      } else {\n        passwordInput.type = 'password';\n        eyeIcon.innerHTML = `\n          <path d=\"M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z\"/>\n          <circle cx=\"12\" cy=\"12\" r=\"3\"/>\n        `;\n      }\n    }\n\n    // Handle Login\n    function handleLogin(event) {\n      event.preventDefault();\n      const email = document.getElementById('email').value;\n      const password = document.getElementById('password').value;\n      \n      if (!email || !password) {\n        alert('メールアドレスとパスワードを入力してください。');\n        return;\n      }\n\n      // デモ仕様: MFAモーダルを開く（シミュレーション）\n      openMfaModal();\n    }\n\n    // MFA Modal Handlers\n    function openMfaModal() {\n      document.getElementById('mfaModal').classList.add('active');\n      document.getElementById('mfa1').focus();\n    }\n\n    function closeMfaModal() {\n      document.getElementById('mfaModal').classList.remove('active');\n    }\n\n    // Auto focus move for MFA inputs\n    function moveFocus(current, index) {\n      if (current.value.length >= 1 && index < 6) {\n        document.getElementById(`mfa${index + 1}`).focus();\n      }\n    }\n\n    // MFA Verification and routing simulation\n    function verifyMFACode() {\n      const mfaCode = [\n        document.getElementById('mfa1').value,\n        document.getElementById('mfa2').value,\n        document.getElementById('mfa3').value,\n        document.getElementById('mfa4').value,\n        document.getElementById('mfa5').value,\n        document.getElementById('mfa6').value\n      ].join('');\n\n      if (mfaCode.length < 6) {\n        alert('6桁の正しいコードを入力してください。');\n        return;\n      }\n\n      const role = document.getElementById('roleSelector').value;\n      closeMfaModal();\n      \n      let destinationName = '';\n      if (role === 'SCR-003') destinationName = '愛好家向けダッシュボード (SCR-003)';\n      if (role === 'SCR-023') destinationName = 'ラボオーナー向けダッシュボード (SCR-023)';\n      if (role === 'SCR-025') destinationName = '管理者管理画面 (SCR-025)';\n\n      alert(`【ログイン成功】\\n認証に成功しました。\\n選択されたロールの画面へ遷移します：\\n${destinationName}`);\n    }\n\n    // Password Reset Demo\n    function triggerPasswordReset() {\n      const email = document.getElementById('email').value || 'sato.hanako@example.co.jp';\n      alert(`パスワード再設定用のメール送信トリガーを実行しました。\\n送信先: ${email}`);\n    }\n\n    // Demo Helper: Fill with preset user\n    function fillDemoUser(email, pass) {\n      document.getElementById('email').value = email;\n      document.getElementById('password').value = pass;\n    }\n\n    // Navigate simulation\n    function navigate(screenId, message) {\n      alert(`画面遷移シミュレーション:\\n画面ID: ${screenId}\\n概要: ${message}`);\n    }\n  </script>\n</body>\n</html>"
    },
    {
      "screenId": "SCR-002",
      "screenName": "会員登録・本人確認画面",
      "category": "アカウント管理",
      "targetUser": "新規一般愛好家ユーザー",
      "overview": "一般のフィルム写真愛好家が、メールアドレスやSNS連携を介して会員登録を行う画面。登録後の仮登録ステータスから、メール内のアクティベーションリンクを経て本登録へ移行させる。",
      "components": [
        {
          "name": "会員登録フォーム",
          "type": "form",
          "description": "メールアドレス、パスワードを入力し利用規約に同意するフォーム"
        },
        {
          "name": "SNSアカウント連携ボタン群",
          "type": "button-group",
          "description": "SNSアカウント情報を流用して簡易的に会員登録を行うためのボタン"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "メールアドレス、パスワードを入力し、利用規約に同意チェックを入れて登録ボタンを押下する",
          "systemResponse": "仮登録完了画面を表示し、指定のメールアドレスに本人確認用メールを送信する。"
        },
        {
          "step": 2,
          "action": "受信した確認用メールのURLをクリックする",
          "systemResponse": "本人確認ステータスを有効（is_verified = true）とし、登録完了通知を画面に表示する。"
        }
      ],
      "fields": [
        {
          "name": "メールアドレス",
          "type": "email",
          "required": true,
          "validation": "RFC規格準拠, 重複不可",
          "description": "連絡先を兼ねるメインのアドレス"
        },
        {
          "name": "パスワード",
          "type": "password",
          "required": true,
          "validation": "半角英大文字・小文字・数字を混在させた8文字以上",
          "description": "強固なパスワードの設定を推奨"
        },
        {
          "name": "利用規約同意フラグ",
          "type": "checkbox",
          "required": true,
          "validation": "必須チェック",
          "description": "プライバシーポリシーおよび利用規約への同意"
        }
      ],
      "events": [
        {
          "trigger": "登録するボタンクリック",
          "action": "仮アカウント作成、アクティベーションメール配信",
          "description": "フォームのバリデーションを実行し、成功時に仮登録APIを呼び出す。"
        }
      ],
      "transitions": [
        {
          "action": "登録完了画面でのログインリンククリック",
          "destination": "SCR-001",
          "condition": "メール確認が完了しアカウントがアクティブな場合"
        }
      ],
      "mockHtml": "<!DOCTYPE html>\n<html lang=\"ja\">\n<head>\n  <meta charset=\"UTF-8\">\n  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n  <title>会員登録・本人確認 | FilmLife</title>\n  <style>\n    :root {\n      --text-main: #1e293b;\n      --text-muted: #475569;\n      --text-light: #94a3b8;\n      --bg-main: #f8fafc;\n      --bg-card: #ffffff;\n      --border-color: #e2e8f0;\n      --primary: #2563eb;\n      --primary-hover: #1d4ed8;\n      --primary-light: #eff6ff;\n      --success: #16a34a;\n      --success-light: #f0fdf4;\n      --error: #dc2626;\n      --warning: #d97706;\n      --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.05);\n      --transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);\n    }\n\n    * {\n      box-sizing: border-box;\n      margin: 0;\n      padding: 0;\n    }\n\n    body {\n      font-family: -apple-system, BlinkMacSystemFont, \"Segoe UI\", Roboto, \"Helvetica Neue\", Arial, sans-serif;\n      background-color: var(--bg-main);\n      color: var(--text-main);\n      min-height: 100vh;\n      display: flex;\n      flex-direction: column;\n      align-items: center;\n      justify-content: center;\n      padding: 24px;\n    }\n\n    .container {\n      width: 100%;\n      max-width: 1000px;\n      background: var(--bg-card);\n      border-radius: 16px;\n      box-shadow: var(--shadow);\n      display: grid;\n      grid-template-columns: 1fr 1.2fr;\n      overflow: hidden;\n      min-height: 640px;\n    }\n\n    /* 左側：ブランドPRエリア */\n    .promo-aside {\n      background: linear-gradient(135deg, #1e3a8a 0%, #0f172a 100%);\n      color: #ffffff;\n      padding: 48px;\n      display: flex;\n      flex-direction: column;\n      justify-content: space-between;\n      position: relative;\n      overflow: hidden;\n    }\n\n    .promo-aside::before {\n      content: '';\n      position: absolute;\n      top: -50%;\n      right: -50%;\n      width: 200%;\n      height: 200%;\n      background: radial-gradient(circle, rgba(37,99,235,0.15) 0%, transparent 60%);\n      pointer-events: none;\n    }\n\n    .brand-logo {\n      display: flex;\n      align-items: center;\n      gap: 10px;\n      font-size: 24px;\n      font-weight: 800;\n      letter-spacing: -0.5px;\n    }\n\n    .brand-logo svg {\n      width: 32px;\n      height: 32px;\n      stroke: #60a5fa;\n    }\n\n    .promo-content h2 {\n      font-size: 28px;\n      font-weight: 700;\n      line-height: 1.4;\n      margin-bottom: 16px;\n    }\n\n    .promo-content p {\n      color: #94a3b8;\n      font-size: 15px;\n      line-height: 1.6;\n    }\n\n    .step-indicator {\n      display: flex;\n      flex-direction: column;\n      gap: 20px;\n      margin-top: 40px;\n    }\n\n    .step-item {\n      display: flex;\n      align-items: center;\n      gap: 12px;\n      opacity: 0.5;\n      transition: var(--transition);\n    }\n\n    .step-item.active {\n      opacity: 1;\n    }\n\n    .step-number {\n      width: 28px;\n      height: 28px;\n      border-radius: 50%;\n      background: rgba(255, 255, 255, 0.1);\n      border: 2px solid rgba(255, 255, 255, 0.2);\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      font-size: 12px;\n      font-weight: 600;\n    }\n\n    .step-item.active .step-number {\n      background: var(--primary);\n      border-color: var(--primary);\n    }\n\n    .step-text {\n      font-size: 14px;\n      font-weight: 500;\n    }\n\n    /* 右側：メインコンテンツエリア */\n    .main-content {\n      padding: 48px;\n      display: flex;\n      flex-direction: column;\n      justify-content: center;\n      position: relative;\n    }\n\n    .screen-view {\n      display: none;\n      animation: fadeIn 0.4s ease forwards;\n    }\n\n    .screen-view.active {\n      display: block;\n    }\n\n    @keyframes fadeIn {\n      from { opacity: 0; transform: translateY(10px); }\n      to { opacity: 1; transform: translateY(0); }\n    }\n\n    .header-group {\n      margin-bottom: 28px;\n    }\n\n    .header-group h1 {\n      font-size: 24px;\n      color: var(--text-main);\n      margin-bottom: 8px;\n    }\n\n    .header-group p {\n      color: var(--text-muted);\n      font-size: 14px;\n    }\n\n    /* SNS 連携ボタン */\n    .sns-group {\n      display: grid;\n      grid-template-columns: 1fr 1fr;\n      gap: 12px;\n      margin-bottom: 24px;\n    }\n\n    .sns-btn {\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      gap: 8px;\n      padding: 10px 16px;\n      border: 1px solid var(--border-color);\n      border-radius: 8px;\n      background: #ffffff;\n      color: var(--text-muted);\n      font-size: 14px;\n      font-weight: 500;\n      cursor: pointer;\n      transition: var(--transition);\n    }\n\n    .sns-btn:hover {\n      background: var(--bg-main);\n      border-color: var(--text-light);\n    }\n\n    .sns-btn svg {\n      width: 18px;\n      height: 18px;\n    }\n\n    .divider {\n      display: flex;\n      align-items: center;\n      text-align: center;\n      color: var(--text-light);\n      font-size: 12px;\n      margin-bottom: 24px;\n    }\n\n    .divider::before, .divider::after {\n      content: '';\n      flex: 1;\n      border-bottom: 1px solid var(--border-color);\n    }\n\n    .divider:not(:empty)::before {\n      margin-right: .5em;\n    }\n\n    .divider:not(:empty)::after {\n      margin-left: .5em;\n    }\n\n    /* フォームパーツ */\n    .form-group {\n      margin-bottom: 20px;\n      position: relative;\n    }\n\n    .form-label {\n      display: block;\n      font-size: 13px;\n      font-weight: 600;\n      color: var(--text-muted);\n      margin-bottom: 6px;\n    }\n\n    .form-label span.required {\n      color: var(--error);\n      margin-left: 4px;\n    }\n\n    .input-wrapper {\n      position: relative;\n    }\n\n    .input-icon {\n      position: absolute;\n      left: 12px;\n      top: 50%;\n      transform: translateY(-50%);\n      color: var(--text-light);\n      pointer-events: none;\n    }\n\n    .input-icon svg {\n      width: 18px;\n      height: 18px;\n    }\n\n    .form-control {\n      width: 100%;\n      padding: 12px 12px 12px 40px;\n      border: 1.5px solid var(--border-color);\n      border-radius: 8px;\n      font-size: 14px;\n      color: var(--text-main);\n      background-color: #ffffff;\n      transition: var(--transition);\n    }\n\n    .form-control:focus {\n      outline: none;\n      border-color: var(--primary);\n      box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.1);\n    }\n\n    /* パスワード表示トグル */\n    .password-toggle {\n      position: absolute;\n      right: 12px;\n      top: 50%;\n      transform: translateY(-50%);\n      background: none;\n      border: none;\n      color: var(--text-light);\n      cursor: pointer;\n      padding: 4px;\n    }\n\n    .password-toggle:hover {\n      color: var(--text-muted);\n    }\n\n    .password-toggle svg {\n      width: 18px;\n      height: 18px;\n    }\n\n    /* チェックボックス */\n    .checkbox-group {\n      display: flex;\n      align-items: flex-start;\n      gap: 10px;\n      margin-top: 24px;\n      margin-bottom: 24px;\n    }\n\n    .checkbox-group input[type=\"checkbox\"] {\n      appearance: none;\n      -webkit-appearance: none;\n      width: 18px;\n      height: 18px;\n      border: 1.5px solid var(--border-color);\n      border-radius: 4px;\n      outline: none;\n      cursor: pointer;\n      position: relative;\n      background: #ffffff;\n      transition: var(--transition);\n      margin-top: 2px;\n    }\n\n    .checkbox-group input[type=\"checkbox\"]:checked {\n      background-color: var(--primary);\n      border-color: var(--primary);\n    }\n\n    .checkbox-group input[type=\"checkbox\"]:checked::before {\n      content: \"\";\n      position: absolute;\n      left: 5px;\n      top: 2px;\n      width: 5px;\n      height: 9px;\n      border: solid white;\n      border-width: 0 2px 2px 0;\n      transform: rotate(45deg);\n    }\n\n    .checkbox-label {\n      font-size: 13px;\n      color: var(--text-muted);\n      line-height: 1.5;\n      user-select: none;\n    }\n\n    .checkbox-label a {\n      color: var(--primary);\n      text-decoration: none;\n      font-weight: 500;\n    }\n\n    .checkbox-label a:hover {\n      text-decoration: underline;\n    }\n\n    /* ボタン関係 */\n    .btn-primary {\n      width: 100%;\n      padding: 14px;\n      background: var(--primary);\n      color: #ffffff;\n      border: none;\n      border-radius: 8px;\n      font-size: 15px;\n      font-weight: 600;\n      cursor: pointer;\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      gap: 8px;\n      transition: var(--transition);\n    }\n\n    .btn-primary:hover {\n      background: var(--primary-hover);\n    }\n\n    .btn-primary:active {\n      transform: scale(0.98);\n    }\n\n    /* 仮登録完了（ステップ2） */\n    .mail-sent-box {\n      background-color: var(--primary-light);\n      border: 1px dashed rgba(37, 99, 235, 0.3);\n      border-radius: 12px;\n      padding: 24px;\n      margin: 24px 0;\n      text-align: left;\n    }\n\n    .mail-info-row {\n      display: flex;\n      margin-bottom: 12px;\n      font-size: 13px;\n    }\n\n    .mail-info-label {\n      width: 80px;\n      font-weight: 600;\n      color: var(--text-muted);\n    }\n\n    .mail-info-val {\n      font-weight: 500;\n      color: var(--text-main);\n    }\n\n    /* シミュレーション用メール開封風カード */\n    .demo-simulation-card {\n      border: 1px solid var(--border-color);\n      border-radius: 12px;\n      padding: 20px;\n      background: #ffffff;\n      margin-top: 24px;\n      box-shadow: 0 4px 12px rgba(0,0,0,0.02);\n    }\n\n    .demo-badge {\n      display: inline-block;\n      background: var(--warning);\n      color: #ffffff;\n      font-size: 10px;\n      font-weight: 700;\n      padding: 2px 8px;\n      border-radius: 12px;\n      margin-bottom: 12px;\n      text-transform: uppercase;\n    }\n\n    /* 本登録完了（ステップ3） */\n    .success-illustration {\n      display: flex;\n      justify-content: center;\n      margin-bottom: 24px;\n    }\n\n    .success-icon-wrap {\n      width: 72px;\n      height: 72px;\n      border-radius: 50%;\n      background-color: var(--success-light);\n      color: var(--success);\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      animation: scaleUp 0.5s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;\n    }\n\n    @keyframes scaleUp {\n      from { transform: scale(0); opacity: 0; }\n      to { transform: scale(1); opacity: 1; }\n    }\n\n    .success-icon-wrap svg {\n      width: 36px;\n      height: 36px;\n    }\n\n    .btn-secondary {\n      width: 100%;\n      padding: 14px;\n      background: var(--bg-main);\n      color: var(--text-main);\n      border: 1px solid var(--border-color);\n      border-radius: 8px;\n      font-size: 15px;\n      font-weight: 600;\n      cursor: pointer;\n      display: flex;\n      align-items: center;\n      justify-content: center;\n      gap: 8px;\n      transition: var(--transition);\n      text-decoration: none;\n    }\n\n    .btn-secondary:hover {\n      background: var(--border-color);\n    }\n\n    /* モバイル対応 */\n    @media (max-width: 768px) {\n      .container {\n        grid-template-columns: 1fr;\n      }\n      .promo-aside {\n        padding: 32px;\n        min-height: 200px;\n      }\n      .main-content {\n        padding: 32px 20px;\n      }\n      .step-indicator {\n        flex-direction: row;\n        margin-top: 24px;\n      }\n      .step-text {\n        display: none;\n      }\n    }\n  </style>\n</head>\n<body>\n\n  <div class=\"container\">\n    <!-- 左側：プロモーション＆進捗インジケーター -->\n    <aside class=\"promo-aside\">\n      <div class=\"brand-logo\">\n        <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2.5\" stroke-linecap=\"round\" stroke-linejoin=\"round\">\n          <path d=\"M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z\"/>\n          <circle cx=\"12\" cy=\"13\" r=\"4\"/>\n        </svg>\n        <span>FilmLife</span>\n      </div>\n\n      <div class=\"promo-content\">\n        <h2>一枚の写真から始まる、<br>新しいつながり。</h2>\n        <p>FilmLifeは、世界中のフィルム写真愛好家が集うコミュニティプラットフォームです。お気に入りの現像レシピやこだわりのカメラ情報をシェアしましょう。</p>\n      </div>\n\n      <!-- ステップインジケーター -->\n      <div class=\"step-indicator\">\n        <div class=\"step-item active\" id=\"step1-indicator\">\n          <div class=\"step-number\">1</div>\n          <div class=\"step-text\">アカウント登録</div>\n        </div>\n        <div class=\"step-item\" id=\"step2-indicator\">\n          <div class=\"step-number\">2</div>\n          <div class=\"step-text\">メール確認</div>\n        </div>\n        <div class=\"step-item\" id=\"step3-indicator\">\n          <div class=\"step-number\">3</div>\n          <div class=\"step-text\">登録完了</div>\n        </div>\n      </div>\n    </aside>\n\n    <!-- 右側：動的メインコンテンツ領域 -->\n    <main class=\"main-content\">\n      \n      <!-- STEP 1: 新規登録入力画面 -->\n      <div class=\"screen-view active\" id=\"view-step1\">\n        <div class=\"header-group\">\n          <h1>新規会員登録</h1>\n          <p>アカウントを作成してギャラリーを始めましょう</p>\n        </div>\n\n        <!-- SNS 連携ボタン -->\n        <div class=\"sns-group\">\n          <button class=\"sns-btn\" type=\"button\">\n            <!-- Google icon placeholder (SVG) -->\n            <svg viewBox=\"0 0 24 24\" fill=\"none\">\n              <path d=\"M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z\" fill=\"#4285F4\"/>\n              <path d=\"M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z\" fill=\"#34A853\"/>\n              <path d=\"M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.06H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.94l2.85-2.22-.03-.63z\" fill=\"#FBBC05\"/>\n              <path d=\"M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.06l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z\" fill=\"#EA4335\"/>\n            </svg>\n            Googleで登録\n          </button>\n          <button class=\"sns-btn\" type=\"button\">\n            <!-- Apple icon placeholder (SVG) -->\n            <svg viewBox=\"0 0 24 24\" fill=\"currentColor\">\n              <path d=\"M18.71 19.5c-.83 1.24-1.71 2.45-3.05 2.47-1.34.03-1.77-.79-3.29-.79-1.53 0-2 .77-3.27.82-1.31.05-2.3-1.32-3.14-2.53C4.25 17 2.94 12.45 4.7 9.39c.87-1.52 2.43-2.48 4.12-2.51 1.28-.02 2.5.87 3.29.87.78 0 2.26-1.07 3.81-.91.65.03 2.47.26 3.64 1.98-.09.06-2.17 1.28-2.15 3.81.03 3.02 2.65 4.03 2.68 4.04-.03.07-.42 1.44-1.38 2.83M15.97 4.17c.66-.81 1.11-1.93.99-3.06-1 .04-2.2.67-2.92 1.49-.62.71-1.16 1.85-1.01 2.96 1.12.09 2.27-.58 2.94-1.39z\"/>\n            </svg>\n            Appleで登録\n          </button>\n        </div>\n\n        <div class=\"divider\">または</div>\n\n        <!-- フォーム本体 -->\n        <form id=\"registerForm\" onsubmit=\"handleSubmit(event)\">\n          <div class=\"form-group\">\n            <label class=\"form-label\">メールアドレス <span class=\"required\">*</span></label>\n            <div class=\"input-wrapper\">\n              <span class=\"input-icon\">\n                <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\"><rect x=\"2\" y=\"4\" width=\"20\" height=\"16\" rx=\"2\"/><path d=\"M22 6L12 13 2 6\"/></svg>\n              </span>\n              <!-- リアルなダミーデータをあらかじめセット -->\n              <input type=\"email\" class=\"form-control\" id=\"emailInput\" required value=\"yamada@example.com\">\n            </div>\n          </div>\n\n          <div class=\"form-group\">\n            <label class=\"form-label\">パスワード <span class=\"required\">*</span></label>\n            <div class=\"input-wrapper\">\n              <span class=\"input-icon\">\n                <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\"><path d=\"M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z\"/></svg>\n              </span>\n              <input type=\"password\" class=\"form-control\" id=\"passwordInput\" required value=\"Yamada1234!\">\n              <button type=\"button\" class=\"password-toggle\" onclick=\"togglePasswordVisibility()\" aria-label=\"パスワード表示切替\">\n                <svg id=\"eye-icon\" viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\"><path d=\"M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z\"/><circle cx=\"12\" cy=\"12\" r=\"3\"/></svg>\n              </button>\n            </div>\n          </div>\n\n          <div class=\"checkbox-group\">\n            <input type=\"checkbox\" id=\"termsCheckbox\" required checked>\n            <label for=\"termsCheckbox\" class=\"checkbox-label\">\n              <a href=\"#\" onclick=\"event.preventDefault();\">利用規約</a> および <a href=\"#\" onclick=\"event.preventDefault();\">プライバシーポリシー</a> に同意します。\n            </label>\n          </div>\n\n          <button type=\"submit\" class=\"btn-primary\">\n            <span>アカウントを作成する</span>\n            <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\" style=\"width: 18px; height: 18px;\"><path d=\"M9 18l6-6-6-6\"/></svg>\n          </button>\n        </form>\n      </div>\n\n      <!-- STEP 2: メール送信完了 ＆ 本人確認シミュレーション -->\n      <div class=\"screen-view\" id=\"view-step2\">\n        <div class=\"header-group\">\n          <h1>仮登録が完了しました</h1>\n          <p>入力されたメールアドレスあてに確認リンクを送信しました。受信トレイをご確認ください。</p>\n        </div>\n\n        <div class=\"mail-sent-box\">\n          <div class=\"mail-info-row\">\n            <div class=\"mail-info-label\">送信先</div>\n            <div class=\"mail-info-val\" id=\"sent-email-address\">yamada@example.com</div>\n          </div>\n          <div class=\"mail-info-row\">\n            <div class=\"mail-info-label\">有効期限</div>\n            <div class=\"mail-info-val\">24時間以内（2024年1月16日 15:30 まで）</div>\n          </div>\n        </div>\n\n        <!-- 本人確認メールのシミュレーションビュー -->\n        <div class=\"demo-simulation-card\">\n          <div class=\"demo-badge\">受信メールのシミュレーション</div>\n          <p style=\"font-size: 13px; font-weight: 600; margin-bottom: 8px; color: var(--text-main);\">\n            【FilmLife】メールアドレスの確認を完了してください\n          </p>\n          <p style=\"font-size: 12px; color: var(--text-muted); margin-bottom: 16px; line-height: 1.5;\">\n            山田太郎 様<br>\n            FilmLifeへのご登録ありがとうございます。以下のボタンをクリックして本登録を完了させてください。\n          </p>\n          <button type=\"button\" class=\"btn-primary\" onclick=\"handleVerification()\" style=\"font-size: 13px; padding: 10px 16px;\">\n            <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\" style=\"width: 16px; height: 16px;\"><path d=\"M20 6L9 17l-5-5\"/></svg>\n            <span>メールアドレスを確認する (is_verified = true)</span>\n          </button>\n        </div>\n      </div>\n\n      <!-- STEP 3: 本登録完了画面 -->\n      <div class=\"screen-view\" id=\"view-step3\">\n        <div class=\"success-illustration\">\n          <div class=\"success-icon-wrap\">\n            <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"3\" stroke-linecap=\"round\" stroke-linejoin=\"round\">\n              <polyline points=\"20 6 9 17 4 12\"></polyline>\n            </svg>\n          </div>\n        </div>\n\n        <div class=\"header-group\" style=\"text-align: center; margin-bottom: 32px;\">\n          <h1 style=\"color: var(--success);\">本登録が完了しました！</h1>\n          <p>本人確認が正常に完了し、FilmLifeアカウントがアクティベートされました。さっそくお気に入りの作品を探索しましょう。</p>\n        </div>\n\n        <div style=\"display: flex; flex-direction: column; gap: 12px;\">\n          <a href=\"#\" class=\"btn-primary\" style=\"text-decoration: none; text-align: center;\" onclick=\"alert('SCR-001：ログイン画面へ遷移します'); return false;\">\n            <span>ログイン画面へ進む (SCR-001)</span>\n            <svg viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\" style=\"width: 18px; height: 18px;\"><path d=\"M9 18l6-6-6-6\"/></svg>\n          </a>\n          <button class=\"btn-secondary\" onclick=\"resetDemo()\">\n            <span>デモをやり直す</span>\n          </button>\n        </div>\n      </div>\n\n    </main>\n  </div>\n\n  <script>\n    // パスワードの表示・非表示切り替え\n    function togglePasswordVisibility() {\n      const pwdInput = document.getElementById('passwordInput');\n      const eyeIcon = document.getElementById('eye-icon');\n      \n      if (pwdInput.type === 'password') {\n        pwdInput.type = 'text';\n        eyeIcon.innerHTML = '<path d=\"M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24\"/><line x1=\"1\" y1=\"1\" x2=\"23\" y2=\"23\"/>';\n      } else {\n        pwdInput.type = 'password';\n        eyeIcon.innerHTML = '<path d=\"M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z\"/><circle cx=\"12\" cy=\"12\" r=\"3\"/>';\n      }\n    }\n\n    // アカウント作成(ステップ1完了)時のイベント\n    function handleSubmit(event) {\n      event.preventDefault();\n      const email = document.getElementById('emailInput').value;\n      document.getElementById('sent-email-address').innerText = email;\n\n      // インジケーターとビューの更新\n      switchView('step2');\n    }\n\n    // 本人確認(ステップ2完了)時のイベント\n    function handleVerification() {\n      // インジケーターとビューの更新\n      switchView('step3');\n    }\n\n    // ビュー切り替えユーティリティ\n    function switchView(targetStep) {\n      // すべてのビューとインジケーターを非アクティブ化\n      document.querySelectorAll('.screen-view').forEach(view => view.classList.remove('active'));\n      document.querySelectorAll('.step-item').forEach(step => step.classList.remove('active'));\n\n      if (targetStep === 'step1') {\n        document.getElementById('view-step1').classList.add('active');\n        document.getElementById('step1-indicator').classList.add('active');\n      } else if (targetStep === 'step2') {\n        document.getElementById('view-step2').classList.add('active');\n        document.getElementById('step1-indicator').classList.add('active');\n        document.getElementById('step2-indicator').classList.add('active');\n      } else if (targetStep === 'step3') {\n        document.getElementById('view-step3').classList.add('active');\n        document.getElementById('step1-indicator').classList.add('active');\n        document.getElementById('step2-indicator').classList.add('active');\n        document.getElementById('step3-indicator').classList.add('active');\n      }\n    }\n\n    // デモを最初に戻す\n    function resetDemo() {\n      switchView('step1');\n    }\n  </script>\n</body>\n</html>"
    },
    {
      "screenId": "SCR-003",
      "screenName": "愛好家ダッシュボード",
      "category": "トランザクション",
      "targetUser": "登録済みの一般愛好家",
      "overview": "ログイン後に表示される愛好家向けのポータル画面。進行中の現像注文、最近納品されたデジタルアーカイブのサムネイル、AIによるパーソナライズされたおすすめ現像ラボ情報などを一覧表示する。",
      "components": [
        {
          "name": "ナビゲーションサイドバー",
          "type": "sidebar",
          "description": "注文履歴、アーカイブ、フリマ、コミュニティへのリンク"
        },
        {
          "name": "進行中注文ステータスカード",
          "type": "card",
          "description": "現在配送中、現像中、検収待ちなどのアクティブ注文を表示する"
        },
        {
          "name": "AI推薦ラボ・フィルムコンポーネント",
          "type": "card",
          "description": "AIが分析したユーザーに最適なラボと推奨フィルム種別をカード形式で提案"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "画面遷移後、自身の配送状況や現像ステータスを俯瞰する",
          "systemResponse": "最新の注文・配送トランザクション状況をリアルタイムで同期しレンダリングする。"
        }
      ],
      "fields": [],
      "events": [
        {
          "trigger": "AIおすすめラボカードのクリック",
          "action": "ラボ詳細画面へ遷移",
          "description": "表示されたAIおすすめの現像ラボの詳細情報を確認するため遷移する。"
        }
      ],
      "transitions": [
        {
          "action": "「現像ラボを探す」ボタンクリック",
          "destination": "SCR-004",
          "condition": "新規で現像注文を行いたい場合"
        },
        {
          "action": "「注文履歴詳細」ボタンクリック",
          "destination": "SCR-008",
          "condition": "進行中の注文情報を追跡したい場合"
        },
        {
          "action": "「アーカイブを見る」メニュークリック",
          "destination": "SCR-010",
          "condition": "過去に納品された写真を見る場合"
        }
      ]
    },
    {
      "screenId": "SCR-004",
      "screenName": "ラボ検索・比較画面",
      "category": "トランザクション",
      "targetUser": "一般愛好家",
      "overview": "GPS（緯度・経度）による位置情報、対応可能なフィルムフォーマット、現像プロセス、料金、ユーザー評価などからラボを絞り込み検索・比較する画面。",
      "components": [
        {
          "name": "検索条件指定パネル",
          "type": "search-box",
          "description": "距離、プロセス（C-41/E-6/モノクロ）、予算などのフィルタリングフォーム"
        },
        {
          "name": "検索結果ラボ一覧テーブル",
          "type": "table",
          "description": "条件に合致する店舗一覧、納期、平均レビュー、基本料金などを表示するテーブル"
        },
        {
          "name": "比較ポップアップトレイ",
          "type": "button-group",
          "description": "複数チェックしたラボ（最大3店舗）を横並びで比較する機能"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "対応フォーマット「120」およびプロセス「モノクロ」を選択して検索する",
          "systemResponse": "ベクトル検索を含む自然言語条件と組み合わせて合致するラボを表示する。"
        },
        {
          "step": 2,
          "action": "複数のラボに比較チェックを入れる",
          "systemResponse": "トレイ上に比較項目（基本料金・納期・クチコミ）を横並びで提示する。"
        }
      ],
      "fields": [
        {
          "name": "位置情報利用許可",
          "type": "checkbox",
          "required": false,
          "validation": "なし",
          "description": "GPS情報を参照して最寄りの店舗を自動計算する"
        },
        {
          "name": "フィルムフォーマット",
          "type": "select",
          "required": false,
          "validation": "なし",
          "description": "35mm, 120, 110, その他"
        },
        {
          "name": "現像プロセス",
          "type": "select",
          "required": false,
          "validation": "なし",
          "description": "C-41, E-6, モノクロ等"
        },
        {
          "name": "セマンティック検索クエリ",
          "type": "text",
          "required": false,
          "validation": "最大100文字",
          "description": "「レトロな暖かい色合い」などの自然言語による要望入力"
        }
      ],
      "events": [
        {
          "trigger": "検索ボタン押下",
          "action": "ラボフィルタリングAPI呼び出し",
          "description": "入力データをクエリパラメータとして送り、ラボデータを抽出する。"
        }
      ],
      "transitions": [
        {
          "action": "ラボ名クリック",
          "destination": "SCR-005",
          "condition": "特定ラボの詳細、メニューを確認したい場合"
        }
      ]
    },
    {
      "screenId": "SCR-005",
      "screenName": "ラボ詳細・サービス一覧画面",
      "category": "トランザクション",
      "targetUser": "一般愛好家",
      "overview": "選択した現像ラボの公式プロフィール、設備状況、専門家からのレビュー（プロビュー）、提供している詳細なサービス（現像・スキャン解像度等）の価格表を確認し、注文を構成する画面。",
      "components": [
        {
          "name": "ラボプロフィールヘッダー",
          "type": "header",
          "description": "店舗ロゴ、紹介、住所、営業時間、平均スコア"
        },
        {
          "name": "サービス価格メニュー表",
          "type": "table",
          "description": "各種フォーマット・プロセスごとの料金、想定納期一覧"
        },
        {
          "name": "レビュー・プロビュー一覧",
          "type": "card",
          "description": "一般レビューに加え、専門家バッジのある信頼性の高い口コミ"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "サービスリストから「35mm / C-41現像 + 高解像度スキャン」を選択する",
          "systemResponse": "選択アイテムと数量を一時セッションカートに追加する。"
        },
        {
          "step": 2,
          "action": "「この内容で現像注文に進む」ボタンを押下する",
          "systemResponse": "カート内の組み合わせをマスタールールでバリデーションし、注文作成画面へ誘導する。"
        }
      ],
      "fields": [
        {
          "name": "数量（本数）",
          "type": "number",
          "required": true,
          "validation": "1以上の整数",
          "description": "現像を希望する物理フィルムの個数"
        }
      ],
      "events": [
        {
          "trigger": "お気に入り店舗に追加",
          "action": "ユーザープロファイルへのラボIDお気に入り保存",
          "description": "次回検索で優先的に表示するためのフラグ登録処理。"
        }
      ],
      "transitions": [
        {
          "action": "注文に進むボタン押下",
          "destination": "SCR-006",
          "condition": "1つ以上のサービスオプションが正常に選択されている場合"
        }
      ]
    },
    {
      "screenId": "SCR-006",
      "screenName": "注文作成・配送選択画面",
      "category": "トランザクション",
      "targetUser": "一般愛好家",
      "overview": "注文する現像サービス詳細、数量を確認し、フィルムの引き渡し方法（店頭直接持込、または提携配送サービスによる集荷）を指定する画面。配送時は自動配送料見積もりが反映される。",
      "components": [
        {
          "name": "注文サービス明細",
          "type": "table",
          "description": "選択された現像プロセスのオプションと数量、小計の確認"
        },
        {
          "name": "引き渡し・返却オプション設定",
          "type": "form",
          "description": "持込方法（店頭/配送）、ネガ返却有無（返送希望/廃棄委託）を指定するフォーム"
        },
        {
          "name": "配送料自動計算プレビュー",
          "type": "card",
          "description": "配送時の現在住所に応じた自動見積り配送料金の表示"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "引き渡し方法として「配送（集荷希望）」を選択し、集荷住所を確認する",
          "systemResponse": "提携配送API（Ahamove/Lalamove等）に問い合わせ、概算配送料金を自動追加・総額を再計算する。"
        },
        {
          "step": 2,
          "action": "ネガ返却について「デジタルスキャン検収後、店舗にて廃棄を委託」を選択する",
          "systemResponse": "返送用配送料をゼロに設定し、最終支払総額を確定する。"
        },
        {
          "step": 3,
          "action": "「オンライン決済へ進む」ボタンをクリックする",
          "systemResponse": "仮注文データを生成（status = pending）し、決済手続きへ進む。"
        }
      ],
      "fields": [
        {
          "name": "フィルム引渡し方法",
          "type": "radio",
          "required": true,
          "validation": "店頭持込 / 配送（集荷） のいずれか",
          "description": "ネガ現物をどうやってラボへ届けるか"
        },
        {
          "name": "集荷先住所",
          "type": "textarea",
          "required": false,
          "validation": "配送選択時のみ必須入力",
          "description": "配送ドライバーがフィルムを預かりに行く場所"
        },
        {
          "name": "ネガフィルム処理方法",
          "type": "radio",
          "required": true,
          "validation": "返送（有料） / 廃棄委託（無料） のいずれか",
          "description": "現像完了後の物理ネガフィルムの処理について"
        }
      ],
      "events": [
        {
          "trigger": "配送住所変更",
          "action": "配送料金再計算API呼び出し",
          "description": "住所文字列変更に伴い緯度経度をデコードし、提携配送の見積もりAPIと非同期通信を行う。"
        }
      ],
      "transitions": [
        {
          "action": "仮注文データの保存と決済への移行",
          "destination": "SCR-007",
          "condition": "入力内容に不整合がない場合"
        }
      ]
    },
    {
      "screenId": "SCR-007",
      "screenName": "オンライン決済画面",
      "category": "トランザクション",
      "targetUser": "一般愛好家（またはフリマ購入者）",
      "overview": "エスクロー（仮受金）を有効にした各種決済ゲートウェイ（クレジットカード、地場決済・モバイルマネー等）を介して安全な決済取引を行う。決済が完了するまで取引金額はプラットフォーム側にプールされる。",
      "components": [
        {
          "name": "注文・取引サマリー",
          "type": "card",
          "description": "決済対象の取引ID、金額、注文内容の表示"
        },
        {
          "name": "決済ゲートウェイ埋め込みセクション",
          "type": "form",
          "description": "クレジットカード情報、モバイルマネーQR決済用セクション"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "クレジットカード情報を入力し、「支払いを確定する」を押下する",
          "systemResponse": "決済完了API（FR-035）をコール。成功時に支払ステータスを「支払済（エスクロー預託中）」へ変更する。"
        }
      ],
      "fields": [
        {
          "name": "カード番号",
          "type": "text",
          "required": true,
          "validation": "Luhnアルゴリズム検証必須",
          "description": "クレジットカードの表面番号"
        },
        {
          "name": "有効期限",
          "type": "text",
          "required": true,
          "validation": "MM/YY 形式、期限内検証",
          "description": "クレジットカードの有効期限"
        },
        {
          "name": "セキュリティコード",
          "type": "password",
          "required": true,
          "validation": "半角数字3〜4桁",
          "description": "CVV/CVCコード"
        }
      ],
      "events": [
        {
          "trigger": "決済実行処理完了",
          "action": "取引手数料差し引き記帳、受注ワークフロー遷移",
          "description": "支払成功通知をWebhookまたはコールバックで検知し、プラットフォーム手数料（FR-037）を算出、該当ラボ宛ての受注承認可能通知をトリガーする。"
        }
      ],
      "transitions": [
        {
          "action": "決済完了（成功）",
          "destination": "SCR-008",
          "condition": "決済トークンが正常に承認された場合"
        }
      ]
    },
    {
      "screenId": "SCR-008",
      "screenName": "注文進捗追跡画面",
      "category": "トランザクション",
      "targetUser": "一般愛好家",
      "overview": "注文したフィルムが現像ラボに届いてからのステータス（受領、現像中、スキャン中、検収待ち等）および、配送中であれば配送会社のリアルタイム追跡情報を視覚的なステップ図で追うことができる画面。",
      "components": [
        {
          "name": "ステータス進捗ゲージ",
          "type": "button-group",
          "description": "現在の進行位置を示すインジケーター（集荷 -> ラボ受領 -> 現像中 -> スキャン中 -> 検収中 -> 完了）"
        },
        {
          "name": "配送GPS追跡地図",
          "type": "card",
          "description": "提携配送API（Ahamove等）のWebhook（FR-027）と同期してドライバーの現在位置を地図表示"
        },
        {
          "name": "ラボ連絡チャットボタン",
          "type": "button-group",
          "description": "疑問がある場合にラボオーナーとチャットを直接開くリンク"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "プッシュ通知またはダッシュボードから遷移し、現像の進捗状況を確認する",
          "systemResponse": "現在のステータス「スキャン中」および、AI自動画質検査結果（CV）の仮異常なしの情報を表示する。"
        }
      ],
      "fields": [],
      "events": [
        {
          "trigger": "ラボステータス更新Webhook受信",
          "action": "UI要素の動的再描画",
          "description": "ラボ側が「スキャン完了・仮納品」を行った瞬間に通知を出し、画面上のボタンを検収確認可能状態へアクティブにする。"
        }
      ],
      "transitions": [
        {
          "action": "「検収画面へ進む」ボタンアクティブ・押下",
          "destination": "SCR-009",
          "condition": "注文ステータスが「検収待ち（仮納品完了）」の場合"
        }
      ]
    },
    {
      "screenId": "SCR-009",
      "screenName": "仮納品プレビュー・検収画面",
      "category": "トランザクション",
      "targetUser": "一般愛好家",
      "overview": "ラボからアップロードされたスキャンデジタルデータを、透かし入り（ウォーターマーク）または低解像度プレビューにて確認し、「検収承認（納品完了・取引完了）」または「不備（再スキャン）申請」を行う画面。",
      "components": [
        {
          "name": "仮納品スライドプレビュー",
          "type": "card",
          "description": "アップロードされた画像の一覧と、拡大・ウォーターマーク表示"
        },
        {
          "name": "検収意思決定パネル",
          "type": "form",
          "description": "「検収承認」または「再スキャン要求（不具合箇所のテキスト指定付き）」ボタン"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "スキャンされた画像を一覧でスクロールし、画質や色合い、大きなホコリ傷がないか検証する",
          "systemResponse": "画像のクリックにより高解像度のウォーターマーク入り画像プレビューを拡大表示する。"
        },
        {
          "step": 2,
          "action": "問題がないことを確認し、「検収承認（取引完了）」ボタンを押下する",
          "systemResponse": "自動取引完了バッチ（FR-025）に取引完了を伝え、エスクローを解除。オリジナル画像のダウンロード権限を付与する。"
        }
      ],
      "fields": [
        {
          "name": "再スキャン希望指摘理由",
          "type": "textarea",
          "required": false,
          "validation": "再スキャン要求選択時のみ必須入力、最大500文字",
          "description": "「〇枚目のピントが合っていない」「右上に著しいノイズ傷がある」等の具体的内容"
        }
      ],
      "events": [
        {
          "trigger": "検収承認ボタンクリック",
          "action": "オリジナル画像のアンロック、売上送金対象確定",
          "description": "保管ステータスを「active」にし、仮納品プレビュー状態から、正式な顧客アーカイブへと登録・反映する。"
        }
      ],
      "transitions": [
        {
          "action": "検収承認を完了する",
          "destination": "SCR-010",
          "condition": "承認に成功し、デジタルアーカイブ画面へ移動を希望する場合"
        }
      ]
    },
    {
      "screenId": "SCR-010",
      "screenName": "デジタルアーカイブ（ギャラリー）画面",
      "category": "レポート",
      "targetUser": "一般愛好家",
      "overview": "検収済みのオリジナル写真高解像度データを、ギャラリー形式でプレビュー、ZIP一括ダウンロードできる画面。カメラ、レンズ、フィルム等のメタデータでの複合フィルタリング、およびアルバムによる整理が可能。",
      "components": [
        {
          "name": "アーカイブ画像ギャラリーグリッド",
          "type": "card",
          "description": "写真のレンダリング、遅延読み込み、ホバー時のメタデータ簡易表示"
        },
        {
          "name": "アーカイブフィルタ・検索サイドバー",
          "type": "sidebar",
          "description": "カメラ、フィルム、撮影年月、カスタムタグなどの複合検索（FR-034）"
        },
        {
          "name": "アーカイブ操作ツールバー",
          "type": "button-group",
          "description": "新規アルバム作成、選択した写真のZIP一括ダウンロード、タグ編集などの一括操作"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "「Portra 400」で撮影された写真をサイドバーでフィルタリングする",
          "systemResponse": "メタデータテーブルを複合インデックス検索し、該当画像を瞬時に絞り込み表示する。"
        },
        {
          "step": 2,
          "action": "対象画像を複数選択し、「ZIPダウンロード」ボタンを押下する",
          "systemResponse": "サーバー側で選択画像を1つのZIPアーカイブに圧縮し、ストリーミングダウンロードを開始する。"
        }
      ],
      "fields": [
        {
          "name": "新規アルバム名",
          "type": "text",
          "required": false,
          "validation": "最大50文字",
          "description": "作成するカスタムアルバムフォルダの名前"
        }
      ],
      "events": [
        {
          "trigger": "ドラッグ＆ドロップによるアルバム分類",
          "action": "写真とアルバムの紐付けリレーション更新",
          "description": "UI上の写真を任意のアルバムカードへドロップした際、DBへ即座に関連付けデータを保存する。"
        }
      ],
      "transitions": [
        {
          "action": "写真単体クリック",
          "destination": "SCR-011",
          "condition": "詳細メタデータ表示・編集を行う場合"
        }
      ]
    },
    {
      "screenId": "SCR-011",
      "screenName": "写真詳細・EXIFメタデータ編集画面",
      "category": "トランザクション",
      "targetUser": "一般愛好家",
      "overview": "選択したデジタル写真の詳細ビュー。自動パースされたEXIF情報の表示に加え、手動で撮影機材、設定値（シャッタースピード、絞り）、撮影地、フィルム銘柄、カスタムタグを追加・編集できる画面。",
      "components": [
        {
          "name": "写真等倍ビューア",
          "type": "card",
          "description": "最大化対応、ロスレスに近い形式でのオリジナルプレビュー"
        },
        {
          "name": "EXIF・カスタムメタデータフォーム",
          "type": "form",
          "description": "撮影機材情報、撮影環境パラメータの編集入力部"
        },
        {
          "name": "タグ追加チップエリア",
          "type": "button-group",
          "description": "タグの新規登録、およびクリックでの削除をサポートするUI"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "EXIFデータにない「使用レンズ: Super Takumar 55mm F1.8」を手動入力して保存する",
          "systemResponse": "メタデータレコード（photo_metadata）を更新し、完了トーストを表示する。"
        }
      ],
      "fields": [
        {
          "name": "カメラ機種名",
          "type": "text",
          "required": false,
          "validation": "最大100文字",
          "description": "撮影に使用したカメラボディ"
        },
        {
          "name": "レンズ機種名",
          "type": "text",
          "required": false,
          "validation": "最大100文字",
          "description": "撮影に使用したレンズ"
        },
        {
          "name": "使用フィルム銘柄",
          "type": "text",
          "required": false,
          "validation": "最大100文字",
          "description": "装填していたフィルム"
        },
        {
          "name": "シャッタースピード",
          "type": "text",
          "required": false,
          "validation": "最大50文字",
          "description": "例: 1/500, 1s等"
        },
        {
          "name": "絞り値",
          "type": "text",
          "required": false,
          "validation": "最大50文字",
          "description": "例: f/1.8, f/8等"
        }
      ],
      "events": [
        {
          "trigger": "メタデータ更新ボタン押下",
          "action": "メタデータテーブル更新APIの実行",
          "description": "フォームデータを検証し、対応するphoto_metadataレコードを永続化する。"
        }
      ],
      "transitions": [
        {
          "action": "一覧へ戻る",
          "destination": "SCR-010",
          "condition": "保存またはキャンセル完了時"
        }
      ]
    },
    {
      "screenId": "SCR-012",
      "screenName": "AI写真技術相談チャット画面",
      "category": "レポート",
      "targetUser": "一般愛好家（全ての愛好家）",
      "overview": "RAG（Retrieval-Augmented Generation）技術を活用し、承認された専門家のナレッジベースやベストアンサー回答から、ユーザーのフィルム写真に関する相談にAIが回答する画面。参考にした専門家記事のURL等も併せて出力される。",
      "components": [
        {
          "name": "チャットタイムライン",
          "type": "card",
          "description": "ユーザーとAIの対話ログ、コード、リンクを含む吹き出し表示"
        },
        {
          "name": "メッセージ入力エリア",
          "type": "search-box",
          "description": "自然言語での質問内容を送信するテキスト入力部と送信ボタン"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "「モノクロフィルムTri-Xを自宅で現像する場合の、D-76現像液の希釈率と標準現像時間を教えて」と入力して送信する",
          "systemResponse": "RAG（FR-058）が作動し、ベクトルストアから最も信頼のおける専門家記事を特定し、手順を要約して回答する。記事への参照リンクを表示する。"
        }
      ],
      "fields": [
        {
          "name": "質問メッセージ",
          "type": "textarea",
          "required": true,
          "validation": "最大1000文字",
          "description": "AIアシスタントに対する任意の技術的な相談クエリ"
        }
      ],
      "events": [
        {
          "trigger": "送信ボタンクリック",
          "action": "RAG/AI返答生成APIの遅延コール",
          "description": "AIによる生成中はタイピングインジケーターアニメーションを表示し、完了後にチャットログに追加する。"
        }
      ],
      "transitions": []
    },
    {
      "screenId": "SCR-013",
      "screenName": "中古機材フリマ検索画面",
      "category": "マスタ管理",
      "targetUser": "フリマ購入希望ユーザー",
      "overview": "ユーザー間で中古カメラ、オールドレンズ、未開封フィルム等を売買できるマーケットプレイスのトップ・検索画面。カテゴリやメーカー、商品のコンディションから探すことができる。",
      "components": [
        {
          "name": "フリマカテゴリタブ",
          "type": "tabs",
          "description": "カメラ / レンズ / フィルム / アクセサリー の切り替え"
        },
        {
          "name": "多角フィルタパネル",
          "type": "search-box",
          "description": "価格帯、状態ランク、出品者の平均レビュー評価での絞り込み"
        },
        {
          "name": "出品商品グリッド",
          "type": "card",
          "description": "サムネイル、タイトル、価格、コンディションを配した製品カード一覧"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "カテゴリ「レンズ」、コンディション「美品」を選択してフィルタする",
          "systemResponse": "利用可能なマーケットプレイス製品（status = available）に限定してデータを再読み込み・描画する。"
        }
      ],
      "fields": [
        {
          "name": "キーワード検索",
          "type": "text",
          "required": false,
          "validation": "最大100文字",
          "description": "製品ブランド、型番など"
        }
      ],
      "events": [
        {
          "trigger": "並び替え順変更",
          "action": "ソート順切り替え（安い順、新着順など）",
          "description": "DBクエリのOrderBy句を書き換えリクエストを実行する。"
        }
      ],
      "transitions": [
        {
          "action": "製品カードクリック",
          "destination": "SCR-014",
          "condition": "該当商品の詳細情報を確認したい場合"
        },
        {
          "action": "「中古機材を出品する」ボタンクリック",
          "destination": "SCR-015",
          "condition": "ログイン済みの愛好家ユーザーで、自身が出品を行う場合"
        }
      ]
    },
    {
      "screenId": "SCR-014",
      "screenName": "フリマ商品詳細画面",
      "category": "トランザクション",
      "targetUser": "フリマ購入希望ユーザー",
      "overview": "中古機材の詳細仕様、コンディション（傷、曇り等の詳細）、出品者プロフィールおよび信頼度評価スコア、複数枚の商品写真を表示する画面。購入に進む、または出品者とのダイレクトチャットを開始できる。",
      "components": [
        {
          "name": "商品フォトギャラリー",
          "type": "card",
          "description": "登録された最大10枚の画像を拡大・切り替え可能で表示するカルーセル"
        },
        {
          "name": "出品者プロフィール要約カード",
          "type": "card",
          "description": "出品者の取引完了実績数、平均レビュー、バッジ情報"
        },
        {
          "name": "購入アクションパネル",
          "type": "button-group",
          "description": "「購入手続きに進む」ボタンと、「出品者に質問する（チャット）」ボタン"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "「購入手続きへ進む」をクリックする",
          "systemResponse": "購入仮確保を行い、決済画面（SCR-007）にエスクロー取引として移行する。"
        },
        {
          "step": 2,
          "action": "「出品者に質問する」をクリックする",
          "systemResponse": "リアルタイム通信が可能なチャット画面を作成、該当ユーザーへのトークスレッドを開く。"
        }
      ],
      "fields": [],
      "events": [
        {
          "trigger": "不適切出品通報ボタンクリック",
          "action": "不適切出品通報（FR-049）",
          "description": "ワンクリックで管理者にガイドライン違反事項を通報する。累積回数に応じて自動非表示モジュールが作動する。"
        }
      ],
      "transitions": [
        {
          "action": "購入手続きボタン押下",
          "destination": "SCR-007",
          "condition": "商品が購入可能な状態（status = available）の場合"
        },
        {
          "action": "チャットボタン押下",
          "destination": "SCR-016",
          "condition": "出品者への問い合わせを開始する場合"
        }
      ]
    },
    {
      "screenId": "SCR-015",
      "screenName": "フリマ機材出品画面",
      "category": "トランザクション",
      "targetUser": "機材出品希望ユーザー（一般愛好家）",
      "overview": "不要になった中古カメラ、レンズ、現像機材等の製品情報、コンディション、販売希望価格を設定し、写真をドラッグ＆ドロップでアップロードしてフリマに出品・公開する登録・編集画面。",
      "components": [
        {
          "name": "出品情報登録フォーム",
          "type": "form",
          "description": "商品タイトル、カテゴリー、製品詳細説明、販売希望価格、中古状態を入力するブロック"
        },
        {
          "name": "マルチ画像ドラッグドロップ領域",
          "type": "file",
          "description": "最大10枚の商品写真をドラッグ＆ドロップで一括アップロードする領域"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "ブランド、型番、商品の詳細状態を記入し、傷のある箇所を含めた写真を5枚アップロードする",
          "systemResponse": "写真の仮アップロードを行い、サムネイル表示する。"
        },
        {
          "step": 2,
          "action": "「この内容で出品を公開する」を押下する",
          "systemResponse": "入力情報を検証し、マーケットプレイス商品テーブル（marketplace_products）にレコード（status = available）を登録する。"
        }
      ],
      "fields": [
        {
          "name": "出品タイトル",
          "type": "text",
          "required": true,
          "validation": "最大255文字, 必須入力",
          "description": "買い手の目を引く商品概要"
        },
        {
          "name": "商品説明文",
          "type": "textarea",
          "required": true,
          "validation": "最大2000文字, 必須入力",
          "description": "動作確認状況、光学系の状態、付属品の有無などを明記"
        },
        {
          "name": "販売希望価格",
          "type": "number",
          "required": true,
          "validation": "1〜9,999,999の整数",
          "description": "売却を希望するプラットフォーム通貨単位の金額"
        },
        {
          "name": "コンディション",
          "type": "select",
          "required": true,
          "validation": "必須選択",
          "description": "新品同様、美品、並品、ジャンクなどの区分"
        }
      ],
      "events": [
        {
          "trigger": "出品確認ボタン押下",
          "action": "フリマ商品レコード新規作成",
          "description": "エラーがない場合に非同期でデータ保存を行い、完了画面を表示する。"
        }
      ],
      "transitions": [
        {
          "action": "出品公開の完了",
          "destination": "SCR-013",
          "condition": "出品登録APIが正常応答を返した場合"
        }
      ]
    },
    {
      "screenId": "SCR-016",
      "screenName": "フリマ取引連絡・チャット画面",
      "category": "トランザクション",
      "targetUser": "フリマ取引の当事者（購入者および出品者）",
      "overview": "Socket.ioによるリアルタイム・ダイレクトチャットを介し、購入前相談や購入決定後の発送方法の調整、追跡番号の伝達、受け取り確認などを進める画面。双方がマイルストーンを完了させる機能を含む。",
      "components": [
        {
          "name": "取引進捗タイムライントラッカー",
          "type": "button-group",
          "description": "購入完了 -> 発送待ち -> 発送完了（追跡番号登録） -> 受取検収完了 の取引合意ステータスを可視化"
        },
        {
          "name": "リアルタイムチャットコンポーネント",
          "type": "card",
          "description": "メッセージ、現物写真、梱包状態の画像等を相互に送り合うエリア"
        },
        {
          "name": "配送追跡情報入力モーダル",
          "type": "modal",
          "description": "出品者が「自家発送」時に、追跡リンクや伝票番号を登録するためのポップアップ"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "出品者が商品を発送後、「発送通知を行う」をクリックして追跡番号を登録する",
          "systemResponse": "取引ステータスを「発送完了（追跡中）」にし、購入者に通知する。チャット欄に自動で追跡リンク付き文言がシステム挿入される。"
        },
        {
          "step": 2,
          "action": "購入者が商品を受け取り、中身を検証して「受取確認（検収）を完了する」をクリックする",
          "systemResponse": "取引ステータスを「検収完了」とし、エスクロー決済金額からシステム手数料を差し引いた金額を出品者の売上残高へ移動する。"
        }
      ],
      "fields": [
        {
          "name": "追跡伝票番号",
          "type": "text",
          "required": true,
          "validation": "半角英数字, 配送オプション連携時のみ",
          "description": "宅配便等の追跡用伝票ナンバー"
        }
      ],
      "events": [
        {
          "trigger": "メッセージ送信",
          "action": "Socket経由のメッセージ同期とデータベースへの会話ログ挿入",
          "description": "非同期で瞬時に対話相手の画面に発言をプッシュ表示させる。"
        }
      ],
      "transitions": [
        {
          "action": "取引完了・相互評価へ移行",
          "destination": "SCR-017",
          "condition": "双方がマイルストーン（受取確認）を完了した場合"
        }
      ]
    },
    {
      "screenId": "SCR-022",
      "screenName": "フィルムラボ加盟申請画面",
      "category": "設定",
      "targetUser": "外部フィルム現像ラボのオーナー",
      "overview": "プラットフォームに未加盟の現像ラボが、店舗住所、対応可能設備、対応プロセス、本人確認資料および事業者営業ライセンスをアップロードし、運営事務局へ加盟申請を出すための画面。",
      "components": [
        {
          "name": "加盟店基本情報入力フォーム",
          "type": "form",
          "description": "ラボ名称、住所、連絡先、緯度経度情報の指定（地図ピッカー付き）"
        },
        {
          "name": "対応サービス・設備設定部",
          "type": "form",
          "description": "対応するフィルム種類、現像プロセスの対応判定チェックボックス群"
        },
        {
          "name": "証明書類一括アップローダー",
          "type": "file",
          "description": "事業者許可証等の公式PDFまたは画像ファイルをアップロードする部分"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "ラボ名、ライセンス情報を入力し、事業者許可証のPDFをアップロードする",
          "systemResponse": "ファイルをバックエンドS3へセキュア保存し、一時仮登録レコードを作成する。"
        },
        {
          "step": 2,
          "action": "「加盟申請を送信する」をクリックする",
          "systemResponse": "申請ステータスを「pending」にして事務局管理者用の承認待ちキューへ送出し、申請受領メールを送信する。"
        }
      ],
      "fields": [
        {
          "name": "ラボ名称",
          "type": "text",
          "required": true,
          "validation": "最大100文字, 必須入力",
          "description": "ユーザーに公開されるラボの正式名称"
        },
        {
          "name": "ラボ公式住所",
          "type": "textarea",
          "required": true,
          "validation": "必須入力",
          "description": "集荷・店頭持込の拠点となる住所"
        },
        {
          "name": "事業者証明書",
          "type": "file",
          "required": true,
          "validation": "PDF, JPG, PNGのみ。10MB以下",
          "description": "公式な営業ライセンス、または本人確認書類"
        }
      ],
      "events": [
        {
          "trigger": "申請送信ボタンクリック",
          "action": "新規申請レコードの生成、管理者通知",
          "description": "バリデーション実施後、labsテーブルに申請中のレコード（status = pending）を挿入する。"
        }
      ],
      "transitions": [
        {
          "action": "申請送信の完了",
          "destination": "SCR-001",
          "condition": "正常に受付完了メッセージが応答された場合"
        }
      ]
    },
    {
      "screenId": "SCR-023",
      "screenName": "ラボ売上ダッシュボード・受注管理画面",
      "category": "トランザクション",
      "targetUser": "加盟フィルムラボのスタッフ・オーナー",
      "overview": "ラボ側のメイン業務画面。日々到着する現像注文の承認・拒否判断、および現在の現像工程（フィルム受領、現像中、スキャン中、納品準備完了）に応じた受注ステータス更新を行うワークフロー管理画面。グラフ付きの売上集計ビュー（ビジネス分析）も内包する。",
      "components": [
        {
          "name": "売上・注文KPIサマリー",
          "type": "header",
          "description": "日次・週次・月次の売上累計、完了注文率、平均処理日数の表示チャート"
        },
        {
          "name": "受注・進捗管理ワークフローテーブル",
          "type": "table",
          "description": "新規注文、作業中の注文をステータスごとにカード・リスト化して俯瞰するカンバンまたはテーブル"
        },
        {
          "name": "売上明細・CSVエクスポートパネル",
          "type": "button-group",
          "description": "税務や会計のための期間指定による売上明細PDF/CSV出力操作部"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "新規に届いた注文カードの「受注を承認する」ボタンをクリックする",
          "systemResponse": "注文ステータスを「承認済」とし、提携配送パートナーAPIへ自動集荷依頼バッチ（FR-026）をフックする。"
        },
        {
          "step": 2,
          "action": "集荷されたフィルムの到着後、作業状況を「スキャン中」に進めるためステータス切り替えトグルを操作する",
          "systemResponse": "該当注文のステータス（status）を更新し、顧客側アプリにリアルタイムでプッシュ進捗通知（FR-019）を送出する。"
        }
      ],
      "fields": [
        {
          "name": "不承認理由フィードバック",
          "type": "text",
          "required": false,
          "validation": "不承認選択時のみ。最大200文字",
          "description": "「対応外の規格フィルムです」等の理由"
        }
      ],
      "events": [
        {
          "trigger": "進捗ステータス切り替え",
          "action": "ステータス更新API呼び出しおよびプッシュ通知同期",
          "description": "データベースの注文ステータスを瞬時に書き換え、対象愛好家の注文進捗トラッカーへ反映する。"
        },
        {
          "trigger": "CSVダウンロード実行",
          "action": "売上・手数料控除明細ファイルの生成・送信",
          "description": "サーバー側で期間集計を行い、ファームバンキングや会計用CSVファイルを生成してブラウザにダウンロードさせる。"
        }
      ],
      "transitions": [
        {
          "action": "「スキャン画像アップロード」をクリック",
          "destination": "SCR-024",
          "condition": "フィルムの現像・乾燥が完了し、高解像度スキャナからデータを入手した場合"
        }
      ]
    },
    {
      "screenId": "SCR-024",
      "screenName": "スキャンデータ一括アップロード画面",
      "category": "トランザクション",
      "targetUser": "加盟フィルムラボのスタッフ",
      "overview": "ラボの作業端末でスキャン完了した複数枚のデジタル画像を、ドラッグ＆ドロップで一括アップロードする画面。注文IDやQRコード情報に基づき、対象顧客（注文）へ画像を自動でマッピング保存し、自動画質評価（CV）を裏で実行する。",
      "components": [
        {
          "name": "一括ドラッグ＆ドロップ画像アップローダー",
          "type": "file",
          "description": "フォルダごと、または複数選択された撮影画像の高速アップロード入力部"
        },
        {
          "name": "アップロード進捗・マッピング確認テーブル",
          "type": "table",
          "description": "ファイル名、紐付け予定の注文ID、自動EXIF解析結果のプレビュー"
        },
        {
          "name": "CV画質評価・アラートバナーエリア",
          "type": "button-group",
          "description": "自動評価エンジン（FR-059）がゴミ、ブレ、ノイズを検知した写真の一覧と要再スキャン警告"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "注文「ORD-9876」用のスキャン済み写真36枚をアップローダーにドロップする",
          "systemResponse": "画像の非同期アップロードを開始。アップロード完了と同時にEXIFデータの自動パースとCV画質評価を実行する。"
        },
        {
          "step": 2,
          "action": "CVスコアが低く「3枚目の画像に過度なブレが検出されました」と赤枠警告バナーが表示されたため、再度対象ネガを再スキャンして上書きアップロードする",
          "systemResponse": "警告バナーが解除され、仮納品準備完了ステータスへ更新される。"
        },
        {
          "step": 3,
          "action": "「顧客へ仮納品を申請する」をクリックする",
          "systemResponse": "透かし（ウォーターマーク）を自動付与したプレビューを顧客の検収画面（SCR-009）へ公開し、通知を送信する。"
        }
      ],
      "fields": [
        {
          "name": "割り当て注文ID",
          "type": "text",
          "required": true,
          "validation": "有効な注文UUID形式",
          "description": "どの注文にこの写真束を紐付けるか"
        }
      ],
      "events": [
        {
          "trigger": "アップロード完了トリガー",
          "action": "CV品質評価エンジンの動作とEXIFの解析",
          "description": "クラウド上のAI/CVサーバーへ画像を引き渡し、解像度、ゴミキズ等の自動一次評価スコア（cv_quality_score）を算出、DBへ保存する。"
        }
      ],
      "transitions": [
        {
          "action": "仮納品処理の完了",
          "destination": "SCR-023",
          "condition": "アップロード・マッピング、仮納品申請がすべて成功した場合"
        }
      ]
    },
    {
      "screenId": "SCR-025",
      "screenName": "システム管理者ポータル画面",
      "category": "設定",
      "targetUser": "システム管理者・モデレーター",
      "overview": "プラットフォーム全体のシステム管理者用ダッシュボード。注文総数、配送リードタイム、決済トラブル率、加盟店舗稼働状況等の主要業務KPIのグラフィカル表示、および各種管理者機能へのナビゲーションポータル。",
      "components": [
        {
          "name": "システムKPIパフォーマンス監視ダッシュボード",
          "type": "card",
          "description": "全プラットフォームの健全性指標、配送ドライバー遅延、決済エラー件数のチャート"
        },
        {
          "name": "管理者メインメニューリスト",
          "type": "sidebar",
          "description": "加盟店審査、記事承認、違反通報、手数料設定メニュー等"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "ダッシュボード上の「加盟ラボ承認待ち（3件）」通知バッジをクリックする",
          "systemResponse": "未処理の加盟申請一覧ページへ遷移する。"
        }
      ],
      "fields": [],
      "events": [],
      "transitions": [
        {
          "action": "加盟ラボ審査を選択する",
          "destination": "SCR-026",
          "condition": "管理者メニューから加盟店舗承認プロセスへ進む場合"
        },
        {
          "action": "通報・モデレーション管理を選択する",
          "destination": "SCR-028",
          "condition": "コミュニティ等の通報一覧を閲覧・処置する場合"
        }
      ]
    },
    {
      "screenId": "SCR-028",
      "screenName": "違反通報・モデレーション管理画面",
      "category": "設定",
      "targetUser": "システム管理者・モデレーター",
      "overview": "コミュニティ投稿、Q&A、中古機材フリマ、ナレッジコメント等でユーザーから寄せられた通報（ガイドライン違反、偽造品出品等）を一覧監視・処理する専用ポータル。",
      "components": [
        {
          "name": "未処理通報アクティブキュー",
          "type": "table",
          "description": "通報日時、通報対象者、通報理由、対象コンテンツのスナップショット、通報回数の一覧"
        },
        {
          "name": "モデレーションアクションダイアログ",
          "type": "modal",
          "description": "該当コンテンツの非表示、アカウント一時凍結（SUSPENDED）、BAN措置、警告送信を決定するインターフェース"
        }
      ],
      "operationSteps": [
        {
          "step": 1,
          "action": "通報回数「5回」の中古カメラ出品レコードを確認し、内容が明らかにガイドラインに違反する不正品コピーと判断する",
          "systemResponse": "該当コンテンツのステータス詳細を表示する。"
        },
        {
          "step": 2,
          "action": "「コンテンツを強制非表示（BAN）にし、出品者に警告メールを送る」を選択して決定する",
          "systemResponse": "即座に商品の公開ステータスを「hidden」にし、検索結果（SCR-013）から除外。対象ユーザーへ自動警告を配信する。"
        }
      ],
      "fields": [
        {
          "name": "処置区分",
          "type": "select",
          "required": true,
          "validation": "必須選択",
          "description": "警告 / コンテンツ削除 / アカウント一時停止 / 強制退会(BAN)"
        },
        {
          "name": "モデレーションメモ",
          "type": "textarea",
          "required": true,
          "validation": "最大1000文字",
          "description": "事務局内部の証拠記録用メモ"
        }
      ],
      "events": [
        {
          "trigger": "処置確定ボタンクリック",
          "action": "アカウント退会・利用停止（BAN）処理API（FR-007）の呼び出し",
          "description": "対象レコードのステータスフラグ（status）を「suspended」または「banned」に書き換える。"
        }
      ],
      "transitions": [
        {
          "action": "処置完了しキューへ復帰",
          "destination": "SCR-025",
          "condition": "操作完了時"
        }
      ]
    }
  ]
}
```