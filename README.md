node-web-server
===

Nodeの勉強のために書いたHTTPサーバです。

- リクエストされたファイルを返します。
- MIME Typeに登録されていないファイルがリクエストされると403 Forbiddenを返します。
- GETとPOSTメソッドに対応しています。
- コマンドラインで実行可能。
- 他のアプリから呼び出し可能。

##インストール - Installation

	$ npm install -g node-web-server

##使用法 - Usage

###Command line

	$ nws localhost:8080

###Script

	var nws = require('nws');
	// 起動
	nws.run({
		host: "localhost",
		port: 8000
	});
	// 10秒後に停止
	setTimeout(nws.stop, 10000);

##設定 - Settings
/lib/http.conf

	{
		"host"			: ホスト名 or IPアドレス or 環境変数(process.env.*),
		"port"			: ポート番号 or 環境変数(process.env.*),
		"docRoot"		: ドキュメントルート(相対パス),
		"defFile"		: [
			デフォルトのインデックスファイル
		],
		"accesslog"		: HTTPリクエストのログファイル(相対パス) or false,
		"errorLog"		: エラー発生時のログファイル(相対パス) or false,
		"httpHeaders"	: {
			HTTPヘッダー
		},
		"MIME"			: {
			MIMEタイプ
		}
	}

ex. Cloud Foundry

	{
		"host"			: "0.0.0.0",
		"port"			: "process.env.VCAP_APP_PORT || 3000",
		<The rest is omitted>

ex. Heroku

	{
		"host"			: "0.0.0.0",
		"port"			: "process.env.PORT || 3000",
		<The rest is omitted>

'||'演算子を使うことができます。
You can use '||' operator.

##今後、追加予定の機能
- ログファイルの閲覧
- アクセス制限
- CGI対応(外部jsの実行)
- HTTPS対応

##ログ - Logging
ログは下記のように記録されます。(アクセスログの例)

	{
		"date":"Fri, Sep 30 2011 20:26:11 GMT-0900",
		"method":"GET",
		"url":"/",
		"statusCode":200
	}
	,{
		"date":"Fri, Sep 30 2011 20:26:11 GMT-0900",
		"method":"GET",
		"url":"style.css",
		"statusCode":200
	}

これを`[]`で囲むことで、JavaScriptの配列として読み込むことが出来ます。

	var obj = JSON.parse("[" + log + "]");


##更新履歴 - History

###v1.1.1: 2012/03/27
- 改行コードをCRLFからLFに変更しました。
- Linuxでnwsコマンドが使えない問題を解決しました。

###v1.1.0: 2012/03/25
- モジュール化し、他のアプリに組み込めるようにしました。
- #4 コマンドラインからの実行も可能。

###v1.0.9: 2012/02/07
- fix #3 URL周りのバグを修正しました。

###v1.0.8: 2012/01/22
- Date Formatの間違いを修正しました。

###v1.0.7: 2012/01/15
- fix #1 URLの扱いを改善しました。
- access.logが生成されないバグを改善しました。

###v1.0.6: 2011/12/19
- Settings Fileでホスティングサービスの環境変数(process.env.*)に対応。

###v1.0.5: 2011/11/12
- Date formatの修正。(RFC1123)

###v1.0.4: 2011/10/07
- デフォルトインデックス(defFile)部分の修正。

###v1.0.3: 2011/09/30
- ロギング部分の修正。

###v1.0.2: 2011/09/26
- エラーメッセージにドキュメントルートのフルパスが表示される脆弱性を修正。

###v1.0.1: 2011/09/02
- bugfix

###v1.0.0: 2011/08/11
- Release