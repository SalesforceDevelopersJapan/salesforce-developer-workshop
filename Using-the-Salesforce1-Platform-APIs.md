---
layout: module
title: モジュール 10&#58; Salesforce1 Platform APIの使用
---
このモジュールでは、Salesforceインスタンスの外部で実行されるアプリケーションを作成します。Salesforceでの認証にはOAuthを使い、REST APIによってSalesforceデータにアクセスするようにします。

![](images/api.jpg)

## 要件

このモジュールの演習を行うには、Node.jsが必要です。Node.jsがお使いのシステムにインストールされていない場合、[こちら](http://nodejs.org/)からインストールできます。

> このモジュールは実施をお勧めしますがオプションです。もしあなたがAPIを使った外部のカスタムアプリケーション(Salesforceインスタンスの外にホストされたアプリケーション)開発に興味がない場合は, モジュール11へ移動して下さい。

## ステップ 1: 接続アプリケーションを作成する

1. **設定** で、 **ビルド** > **作成** > **アプリケーション** の順にクリックします

1. **接続アプリケーション** セクションで **新規** をクリックして、次のように新しい接続アプリケーションを定義します:
  - 接続アプリケーション名: MyConference
  - API参照名: MyConference
  - 取引先責任者 メール: 自身のメールアドレスを入力します
  - OAuth設定の有効化: チェックする
  - コールバックURL: http://localhost:3000/oauthcallback.html
  - 選択したOAuth範囲: フルアクセス (full)

    ![](images/connected-app.jpg)

1. **保存** をクリックします


## ステップ 2: 関連ファイルをインストールする

1. [このファイル](https://github.com/salesforcedevelopersjapan/salesforce-developer-workshop/archive/master.zip)をダウンロードして解凍するか、[このリポジトリ](https://github.com/salesforcedevelopersjapan/salesforce-developer-workshop)からコピーを取得します。

1. コードエディタ（各自、使い慣れたものを使用してください）で、 **client/index.html** 内のコードを確認します:
    - これは上記のスクリーンショットの様な基本的なセッションのリストを表示するマークアップを提供します。
    - ratchet.cssが使われています。[Ratchet](http://goratchet.com/)は、モバイルアプリケーション用のスタイルを提供するシンプルなCSSツールキットです。
    - ここではSalesforceとの統合に [ForceTK](https://github.com/developerforce/Force.com-JavaScript-REST-Toolkit) というForce.com JavaScript REST Toolkitが使われています。
    - 認証(OAuth login) とデータアクセスロジックをjs/app.js に実装していきます。現在空となっています。  

1. コードエディタで、 **client/oauthcallback.html** 内のコードを確認します:

    OAuthワークフローの最後には、Salesforce 認証プロセスが接続アプリケーションに定義したリダイレクトURIをロードし、アクセストークﾝ及びその他のOAuth関連の値(サーバインスタンス, リフレッシュトークン, など) をクエリ文字列を使って渡します。 リダイレクトURIページは単純にクエリ文字列を解析し、アクセストークン及びその他のOAuthの値を展開し、それらの値の情報をステップ4で実装するoauthCallback()関数を呼び出す事によってアプリケーショｎへ返します。

1. コードエディタで、 **server.js** 内のコードを確認します。server.jsは、次の2つの機能を提供する小さなHTTPサーバを実装しています:
    - 静的コンテンツ用のWebサーバ。Webサーバのドキュメントルートは、clientディレクトリです
    - SalesforceのRESTリクエスト用のプロキシ。ブラウザでのクロスオリジンの制限により、お使いのサーバ（またはローカルホスト）がホストするJavaScriptアプリケーションから、*.salesforce.comドメインに対してAPIコールを直接実行することはできません。そこで、プロキシを介在させることによって、サーバからのAPIコールを実行します

## ステップ 3: Node.jsサーバを起動する


1. ターミナル（Macの場合）、またはコマンドプロンプト（Windowsの場合）を起動します

1. salesforce-developer-workshop ディレクトリに移動（cd）します。

1. Node.jsサーバの依存関係をインストールします:

    ```
    npm install
    ```

1. サーバを起動します:  

    ```
    node server
    ```

1. アプリケーションをテストします。ブラウザを開、以下のURLにアクセスします:

    ```
    http://localhost:3000
    ```

    Salesforceへの認証を行わない間、この段階ではセッションリストは空の状態になっています。

## ステップ 4: OAuthを使ったSalesforce認証を実装する

1. コードエディタで **salesforce-developer-workshop/client/js**にある **app.js** を開きます。

1. 以下の変数を宣言します:

    ```
    var apiVersion = 'v30.0',
    clientId = 'YOUR_CONSUMER_KEY',
    loginUrl = 'https://login.salesforce.com/',
    redirectURI = 'http://localhost:3000/oauthcallback.html',
    proxyURL = 'http://localhost:3000/proxy/',
    client = new forcetk.Client(clientId, loginUrl, proxyURL);
    ```

1. Salesforceアプリケーションに戻り、**設定** で、 **ビルド** > **作成** > **アプリケーション** の順にクリックします。 **接続アプリケーション** セクションで、 **MyConference** をクリックし、コンシューマ鍵をクリップボードにコピーします。

    ![](images/consumer-key.jpg)

1. app.jpで、「YOUR_CONSUMER_KEY」の部分を、クリップボードにコピーしたコンシューマ鍵で置き換えます。

1. app.jsで、**login()** という名前の関数を以下の様に実装します(変数宣言のすぐ後):

    ```
    function login() {
        var url = loginUrl + 'services/oauth2/authorize?display=popup&response_type=token' + '&client_id=' + encodeURIComponent(clientId) + '&redirect_uri=' + encodeURIComponent(redirectURI);
        window.open(url);
    }
    ```

1. **oauthCallback()** という名前の関数を以下の様に実装します (login() 関数のすぐ後):

    ```
    function oauthCallback(response) {
        if (response && response.access_token) {
            client.setSessionToken(response.access_token,
                                   apiVersion,
                                   response.instance_url);
            console.log('OAuth authentication succeeded');
        } else {
            alert("AuthenticationError: No Token");
        }
    }
    ```

    > oauthCallback() はoauthcallback.htmlページより
    、 OAuthワークフローの最後に呼び出されます。(oauthcallback.htmlの詳細はステップ2に書いてあります)

1. login() 関数をapp.jsファイルの最後で呼び出します:

    ```
    login();
    ```

1. アプリケーションをテストします
  - ブラウザを開き、  [http://localhost:3000](http://localhost:3000) へアクセスします
  - Developer Editionのログイン情報を使ってログインします。Salesforceでの認証に成功すると、**OAuth authentication succeeded** メッセージが表示されます。次のステップで、REST APIコールを実行してセッションのリストを表示させます

  > 接続アプリケーションは、作成後、利用可能になるまで数分程度かかることがあります。次のようなメッセージが表示された場合は、数分間待ってから再試行してください: **error=invalid_client_id&error_description=client%20identifier%20invalid**

## ステップ 5: REST APIを使用する

1. 1.	client/js/app.jsで、次のようにgetSessions()関数を実装します(oauthCallback()関数のすぐ後):

    ```
    function getSessions() {
        var soql = "SELECT Id, Name, Session_Date__c FROM Session__c",
            html = '';
        client.query(soql,
            function (data) {
                var sessions = data.records;
                for (var i=0; i<sessions.length; i++) {
                    html += '<li class="table-view-cell">' + sessions[i].Name + '</li>';
                }
                $('.session-list').html(html);
            },
            function (error) {
                alert("Error: " + JSON.stringify(error));
            });
        return false;
    }
    ```

1. oauthCallback() 関数を変更し、認証が成功したタイミングで getSessions() を呼び出すように変更します

    ```
    console.log('OAuth authentication succeeded');
    getSessions();
    ```

1. 3.	アプリケーションをテストします
  - ブラウザを開き、  [http://localhost:3000](http://localhost:3000) へアクセスします
  -　Developer Editionのログイン情報を使ってログインします
  - 任意のセッションをクリックし、詳細を確認してみましょう

> 今回のモジュールでは、JavaScriptを使ったカスタムアプリケーションの開発、OAuthによるSalesforceでの認証、REST APIを介したSalesforceデータへのアクセス等に関する基本的な手順のみを取り上げました。このアーキテクチャにもとづいて実際のアプリケーションを開発する予定がある場合は、[Backbone.js](http://backbonejs.org/)、[AngularJS](https://angularjs.org/)ベースの[Ionic](http://ionicframework.com/)等のJavaScriptフレームワークの利用を検討してください。



<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Using-JavaScript-in-Visualforce-Pages.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 戻る</a>
<a href="Testing.html" class="btn btn-default pull-right">次へ <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
