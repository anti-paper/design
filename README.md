# 要件定義書
## 1.アプリケーション名
|||
|--|--|
|正式名称|Muscle Management|
|日本語名称|マッスルマネージメント|
|略称|ますまね|
## 2.アプリケーション種類
- Web
- Android
- iOS
## 3.ロゴ
- 筋肉の絵文字に「ますまね」の文字を組み合わせたような画像
- スマホアプリ版ではアプリアイコンとしても使用
## 4.目的
- GASとスプレッドシートで作ったWebアプリ使ってたけどデータいじる処理が絶妙に重たいのでサクサク動くもの作りたい
- あと開発の練習も兼ねてる
## 5.機能要件
### 5-1.概要
認証機能付き筋トレ管理表
### 5-2.詳細
#### 5-2-1.共通情報
##### 5-2-1-1.トークン
- ランダム文字列
- 255文字
- 有効期限
  - 5分後（タイムスタンプ）
  - 環境変数にて管理
- 有効期限チェック
  - API呼び出しの都度
    - 有効期間内
      - 旧トークン削除（ハードデリート）
      - 新トークン発行
    - 有効期間経過後
      - トークン削除（ハードデリート）
##### 5-2-1-2.削除
- ソフトデリート
  - 削除済みデータ保持期限
    - 1年後（タイムスタンプ）
    - 環境変数にて管理
  - 保持期間経過前
    - 復活可能
  - 保持期間経過後
    - ハードデリート
##### 5-2-1-3.バリデーション
- ルール
  - ユーザ名
    - ユニーク
      - 登録時・ユーザ名変更時
      - アクティブユーザのみ（削除フラグ立ってるユーザとは重複OK）
      - 削除フラグ立ってるユーザとの重複は復活処理のため別で監視
    - 必須
    - 20文字以内
    - 文字列
  - パスワード
    - 必須
    - 8文字以上
    - 255文字以内
    - 数字・文字・記号をそれぞれ1文字以上含む
  - パスワード（確認用）
    - 必須
    - パスワードと同値
    - 8文字以上
    - 255文字以内
    - 数字・文字・記号をそれぞれ1文字以上含む
  - 筋トレメニュー
    - ユニーク
      - 登録時のみ
      - ユーザ毎（別ユーザなら重複OK）
    - 必須
    - 15文字以内
    - 文字列
- バリデーションエラー時のメッセージ（優先順位順）
  - 「必須項目です」
    - 必須
  - 「～文字以上入力してください」
    - ～文字以上
  - 「～文字までしか入力できません」
    - ～文字以下
  - 「文字を1字以上含めてください」
    - 文字列
  - 「数字・文字・記号をそれぞれ1字以上含めてください」
    - 数字・文字・記号をそれぞれ1字以上含む
  - 「パスワードが間違っています」
    - パスワードと同値
  - 「既存のデータと重複しています」
    - ユニーク
#### 5-2-2.画面
- ヘッダ
  - UI
    - 画像
      - ロゴ（小）
    - ボタン
      - ユーザ名
      - 「ユーザ情報」
      - 「ログアウト」
      - 「ユーザ削除」
  - 機能
    - ログアウト
    - ユーザ削除
  - 画面遷移
    - ユーザ情報
    - トップ
- ユーザ情報
  - UI
    - ラベル
      - 「ユーザ名：」ユーザ名
      - 「パスワード：」「*」×パスワード文字数
      - 「筋トレメニュー入力方法：」筋トレメニュー入力方法
      - 「作成日：」作成日
      - 「更新日：」更新日
    - ボタン
      - 「ユーザ名変更」
      - 「パスワード変更」
      - 「ホーム画面に戻る」
  - 機能
    - ユーザ情報取得
  - 画面遷移
    - ユーザ名変更
    - パスワード変更
    - ホーム
- ユーザ名変更
  - UI
    - ラベル
      - 「新しいユーザ名」
    - インプットボックス
      - ユーザ名
    - ボタン
      - 「変更」
      - 「キャンセル」
  - 機能
    - ユーザ名変更
  - 画面遷移
    - ユーザ情報
- パスワード変更
  - UI
    - ラベル
      - 「パスワード」
      - 「新しいパスワード」
      - 「新しいパスワード（確認用）」
    - インプットボックス
      - パスワード
      - 新しいパスワード
      - 新しいパスワード（確認用）
    - ボタン
      - 「変更」
      - 「キャンセル」
  - 機能
    - パスワード変更
  - 画面遷移
    - ユーザ情報
- トップ
  - UI
    - 画像
      - ロゴ（大）
    - ボタン
      - 「ユーザ登録」
      - 「ログイン」
  - 画面遷移
    - ユーザ登録
    - ログイン
- ユーザ登録
  - UI
    - ラベル
      - 「ユーザ名」
      - 「パスワード」
      - 「パスワード（確認用）」
    - インプットボックス
      - ユーザ名
      - パスワード
      - パスワード（確認用）
    - ボタン
      - 「登録」
      - 「キャンセル」
  - 機能
    - ユーザ登録
    - ユーザ情報取得
  - 画面遷移
    - トップ
    - ホーム
- ログイン
  - UI
    - ラベル
      - 「ユーザ名」
      - 「パスワード」
    - インプットボックス
      - ユーザ名
      - パスワード
    - ボタン
      - 「ログイン」
      - 「キャンセル」
  - 機能
    - ログイン
    - ユーザ情報取得
    - ユーザ復活
  - 画面遷移
    - トップ
    - ホーム
- ホーム
  - UI
    - ラベル
      - 「メニュー：」筋トレメニュー
      - 「回数：」筋トレ回数
    - インプットボックス
      - 日付
    - ボタン
      - 「記録」
      - 「過去の記録を見る」
      - 「メニュー編集」
      - 「メニュー順番編集」
      - 「メニュー入力方法編集」
      - 「回数変更」
  - 機能
    - 筋トレ内容記録
    - 筋トレメニュー取得
    - 筋トレメニュー入力方法取得
    - 筋トレ回数取得
  - 画面遷移
    - 筋トレ記録一覧
    - 筋トレメニュー
    - 筋トレメニュー入力方法
    - 筋トレ回数変更
- 筋トレ記録一覧
  - UI
    - テーブル
      - 筋トレ記録
    - ラベル
      - 「～」
    - インプットボックス
      - 開始日
      - 終了日
    - ボタン
      - 「削除」
      - 「次へ」
      - 「前へ」
      - ページ番号
      - 「データ出力」
      - 「キャンセル」
      - 「CSVで出力する」
      - 「ホーム画面に戻る」
  - 機能
    - 筋トレ記録取得
    - 筋トレ記録削除
    - データ出力
  - 画面遷移
    - ホーム
- 筋トレメニュー
  - UI
    - テーブル
      - 筋トレメニュー一覧
    - ラベル
      - 「追加するメニュー」
      - 「入力方法を『登録したメニューを使用（ローテーション）』に設定した時に、「順番」で設定した順番通りにローテーションします」
    - インプットボックス
      - 追加するメニュー
      - 変更するメニュー
      - 順番
    - ボタン
      - 「登録」
      - 「変更」
      - 「更新」
      - 「キャンセル」
      - 「削除」
      - 「ホーム画面に戻る」
  - 機能
    - 筋トレメニュー取得
    - 筋トレメニュー登録
    - 筋トレメニュー更新
    - 筋トレメニュー削除
  - 画面遷移
    - ホーム
- 筋トレメニュー入力方法
  - UI
    - ラベル
      - 「メニューを登録していないと利用できません」
    - ラジオボタン
      - 「直接入力」
      - 「登録したメニューを使用（ランダム）」
      - 「登録したメニューを使用（ローテーション）」
    - ボタン
      - 「変更」
      - 「ホーム画面に戻る」
  - 機能
    - 筋トレメニュー入力方法取得
    - 筋トレメニュー入力方法更新
  - 画面遷移
    - ホーム
- 筋トレ回数変更
  - UI
    - ラベル
      - 「回」
    - インプットボックス
      - 筋トレ回数
    - ボタン
      - 「変更」
      - 「ホームに戻る」
  - 機能
    - 筋トレ回数取得
    - 筋トレ回数更新
  - 画面遷移
    - ホーム
#### 5-2-3.機能
|タグ|機能|リクエスト|レスポンス|メソッド|
|--|--|--|--|--|
|auth|ユーザ登録|・ユーザ名<br>・パスワード|・ユーザID<br>・トークン<br>・筋トレメニュー入力方法|POST|
|auth|ログイン|・ユーザ名<br>・パスワード|・ユーザID<br>・トークン<br>・筋トレメニュー入力方法|PUT|
|auth|ユーザ復活|・ユーザ名<br>・パスワード|・ユーザID<br>・トークン<br>・筋トレメニュー入力方法|PUT|
|auth|ログアウト|・ユーザID<br>・トークン|-|DELETE|
|auth|ユーザ削除|・ユーザID<br>・トークン|-|DELETE|
|users|全ユーザ情報取得|-|・ユーザ名|GET|
|users|ユーザ情報取得|・ユーザID<br>・トークン|・ユーザ名<br>・パスワード文字数<br>・筋トレメニュー入力方法<br>・作成日<br>・更新日<br>・トークン|GET|
|users|ユーザ情報変更|・ユーザID<br>・トークン<br>・ユーザ名<br>・パスワード<br>・筋トレメニュー入力方法|・トークン|PUT|
|logs|筋トレ記録取得|・ユーザID<br>・トークン|・筋トレメニュー<br>・筋トレ日付<br>・筋トレ回数<br>・トークン|GET|
|logs|筋トレ内容記録|・ユーザID<br>・筋トレメニュー<br>・筋トレ日付<br>・筋トレ回数<br>・トークン|・トークン|POST|
|logs|データ出力|・ユーザID<br>・トークン<br>・開始日<br>・終了日|・トークン|GET|
|logs|筋トレ記録削除|・ユーザID<br>・ログID<br>・トークン|・トークン|DELETE|
|menus|筋トレメニュー取得|・ユーザID<br>・トークン|・メニューID<br>・筋トレメニュー<br>・筋トレメニュー順番<br>・トークン|GET|
|menus|筋トレメニュー登録|・ユーザID<br>・トークン<br>・筋トレメニュー<br>・筋トレメニュー順番|・トークン|POST|
|menus|筋トレメニュー変更|・ユーザID<br>・メニューID<br>・トークン<br>・筋トレメニュー<br>・筋トレメニュー順番|・トークン|PUT|
|menus|筋トレメニュー削除|・ユーザID<br>・メニューID<br>・トークン|・トークン|DELETE|
|times|筋トレ回数取得|・ユーザID<br>・トークン|・筋トレ回数<br>・トークン|GET|
|times|筋トレ回数変更|・ユーザID<br>・トークン<br>・筋トレ回数|・トークン|PUT|
<br>

#### 5-2-4.データ
- ユーザ
  - テーブル名
    - users
  - カラム（型）
    - id(bigIncrement)
    - name(string)
    - password(string)
    - method(int)
    - delete_flag(boolean)
    - created_at(timestamp)
    - updated_at(timestamp)
    - deleted_at(timestamp)
  - 主キー
    - id
  - リレーション
    - id
      - tokens_user_id
        - 一対多
      - logs_user_id
        - 一対多
      - menus_user_id
        - 一対多
      - times_user_id
        - 一対一
- トークン
  - テーブル名
    - tokens
  - カラム（型）
    - id(bigIncrement)
    - token(string)
    - user_id(bigInt)
    - created_at(timestamp)
    - expired_at(timestamp)
  - 主キー
    - id
  - リレーション
    - user_id
      - users_id
        - 多対一
- 筋トレ記録
  - テーブル名
    - logs
  - カラム（型）
    - id(bigIncrement)
    - log_date(date)
    - menu(string)
    - times(int)
    - user_id(bigInt)
    - delete_flag(boolean)
    - created_at(timestamp)
    - deleted_at(timestamp)
  - 主キー
    - id
  - リレーション
    - user_id
      - users_id
        - 多対一
- 筋トレメニュー
  - テーブル名
    - menus
  - カラム（型）
    - id(bigIncrement)
    - menu(string)
    - order(int)
    - user_id(bigInt)
    - delete_flag(boolean)
    - created_at(timestamp)
    - updated_at(timestamp)
    - deleted_at(timestamp)
  - 主キー
    - id
  - リレーション
    - user_id
      - users_id
        - 多対一
- 筋トレ回数
  - テーブル名
    - times
  - カラム（型）
    - id(bigIncrement)
    - times(int)
    - user_id(bigInt)
    - delete_flag(boolean)
    - updated_at(timestamp)
    - deleted_at(timestamp)
  - 主キー
    - id
  - リレーション
    - user_id
      - users_id
        - 一対一
## 6.非機能要件
- 管理ツール
  - タスク管理
    - Trello
  - ソース管理
    |開発単位|GitHubリポジトリ名|
    |--|--|
    |API|muscleApi|
    |Web|muscleWeb|
    |Android|muscleAndroid|
    |iOS|muscleIos|
- 要件定義・設計（GitHubリポジトリ「design」に成果物をまとめる）
  |資料名|ツール|成果物|
  |--|--|--|
  |要件定義書|Visual Studio Code|README.md|
  |画面設計書|Draw.io Integration(Visual Studio Code)|display.drawio<br>display-画面名.svg|
  |API設計書|OpenAPI Editor(Visual Studio Code)|api.yaml|
  |DB定義書|A5:SQL Mk-2|db.xlsx|
- テスト
  - テスト環境
    - staging
  - テスト対象
    - Web版
      - Edge
      - Chrome
      - Firefox
      - Safari
    - Android版
      - Android 12
    - iOS版
      - iOS 16
  - テスト項目書
    - どのデータを使用し、どの順番でテストするかも記載
    - クライアント側（画面表示確認）
      - 画面上の操作にて確認
      - 必要に応じてテスト用コマンドを使用
    - サーバ側（データ確認）
      - テストコードを使用
    - テスト結果は独立したシートにて集計する
  - テスト結果報告書
    - 記載内容
      - テスト実施者
      - テスト実施日
      - テスト項目書のリンク
      - テスト結果
        - テスト項目書の集計シートの内容をコピペ
    - エラーが発生したものについては再度テストし直し
      - エラー発生なしの項目はそのままコピペOK
      - エラー発生箇所は修正後の状態で確認
      - 最終的に全項目OKの状態にする
  - 障害報告書
    - テストの結果エラーになったものについて記載していく
  - 記載内容
    - エラー発生時
    - テスト実施者
    - テスト実施日
    - エラー内容
    - テスト項目書上の該当箇所のリンク
    - ステータス
      - 「未着手」を選択
  - 修正対応中
    - 対応者
    - 対応開始日
    - 対応者コメント
    - 修正対象（要件定義書）
      - true又はfalse
    - 修正対象（画面設計書）
      - true又はfalse
    - 修正対象（API設計書）
      - true又はfalse
    - 修正対象（DB定義書）
      - true又はfalse
    - ステータス
      - 「対応中」を選択
  - 修正対応後
    - 対応完了日
    - ステータス
      - 「確認待ち」を選択
  - 修正確認後
    - 確認者
    - 確認日
    - ステータス
      - 「クローズ」を選択
- 開発情報
  - インフラ情報
    - 利用サービス
      - Amazon Web Service
        - よくわからないので詳細は後日記載予定
    - ドメイン
      |環境|Web<br>API|ドメイン|
      |--|--|--|
      |開発環境|Web|https://dev-muscle-management.com/|
      |開発環境|API|https://dev-api-muscle-management.com/|
      |ステージング環境|Web|https://staging-muscle-management.com/|
      |ステージング環境|API|https://staging-api-muscle-management.com/|
      |本番環境|Web|https://muscle-management.com/|
      |本番環境|API|https://api-muscle-management.com/|
    - ポート番号
      |Web<br>API|種別|ポート番号|
      |--|--|--|
      |API|アプリ|9000|
      |API|PhpMyAdmin|9001|
      |Web|-|9010|
    - アクセス許可（セキュリティの観点からIPアドレスは省略）
      |クライアント<br>サーバ|環境|許可内容|
      |--|--|--|
      |クライアント|開発環境|・Windows(Windows 11)<br>・Mac(Ventura 13.1)<br>・Android sense 4 light(Android 12)<br>・iPhone 8(iOS 16)|
      |クライアント|ステージング環境|・Windows(Windows 11)<br>・Mac(Ventura 13.1)<br>・Android sense 4 light(Android 12)<br>・iPhone 8(iOS 16)|
      |クライアント|本番環境|全て許可|
      |サーバ|開発環境|AWSアカウント「anti-paper」|
      |サーバ|ステージング環境|AWSアカウント「anti-paper」|
      |サーバ|本番環境|AWSアカウント「anti-paper」|
    - ソース管理
      - AWSサービス利用（詳細よくわからないので後日記載予定）
    - バッチジョブ管理
      - AWSサービス利用（詳細よくわからないので後日記載予定）
    - バックアップ
      |対象|種別|実行タイミング|実行方法|頻度|保管場所|保管世代数|
      |--|--|--|--|--|--|--|
      |マシン|-|AM 0:00|自動|日次|Amazon S3|7|
      |DB|増分|AM 0:00|自動|日次（日曜除く）|Amazon S3|6|
      |DB|フル|AM 0:00|自動|週次（日曜）|Amazon S3|3|
    - リストア
      - 手順書作成する
        - テスト
        - 環境
          - staging
        - 頻度
          - 月次（毎月末）
        - 対象
          - マシンイメージ（環境変数含む）
          - DB
        - 検証内容
          - マシンイメージ検証（Web, Android, iOSそれぞれで任意の1環境のみ確認でOK）
            - 3形態（Web, Android, iOS）でアプリが起動できること
            - アプリの全機能が利用できること（API及びクライアントのソースが完全に復元できていること。データ変更を伴う機能の検証は必ず下記検証用のstaging環境のデータ取得後に実施する。テスト項目書に沿って検証する）
          - データ検証
            - 事前に本番環境のデータを取得しておき、リストアしたデータと差異がないことを確認する
    - 障害対応
      - メールでの問い合わせのみ受け付ける（問い合わせ用メールアドレスはアプリ内に記載する。「こちら」のような文字列にメールアドレスのリンクを乗せる）
      - 問い合わせ内容を障害一覧に転記
        - 記載内容
          - エラー発生時
            - 問い合わせ実施者メールアドレス
            - 問い合わせ実施日
            - 障害内容
            - ステータス
              - 「未着手」を選択
          - 修正対応中
            - 対応者
            - 対応開始日
            - 対応者コメント
            - 修正対象（要件定義書）
              - true又はfalse
            - 修正対象（画面設計書）
              - true又はfalse
            - 修正対象（API設計書）
              - true又はfalse
            - 修正対象（DB定義書）
              - true又はfalse
            - ステータス
              - 「対応中」を選択
          - 修正対応後（修正リリース後にユーザに完了通知する（文言はテンプレそのまま使用））
            - 対応完了日
            - ステータス
              - 「確認待ち」を選択
          - 通知から1週間経過後（同障害に関する追加の問い合わせなければ）
            - クローズ日
            - ステータス
              - 「クローズ」を選択
            - Ref
              - 「-」と記載
          - 通知から1週間以内に同障害に関する追加の問い合わせあった場合
            - クローズ日
              - 空欄
            - ステータス
              - 「クローズ」を選択
            - Ref
              - 追加問い合わせ分を記載した障害管理Noを記載
          - 備考欄は必要に応じて適宜記載
    - 後日追記予定
- 開発ツール等
  |開発単位|エディタOR開発ツール|言語|フレームワーク|検証ツール|
  |--|--|--|--|--|
  |API|Visual Studio Code|PHP|Laravel(sail)|・REST Client(Visual Studio Code)<br>・Postman|
  |Web|Visual Studio Code|JavaScript|Vue.js|-|
  |Android|Android Studio|Java|-|-|
  |iOS|Xcode|Swift|-|-|
