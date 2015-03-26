---
layout: module
title: モジュール 3&#58; アプリケーションの作成
---
このモジュールでは、セッション及び講演者オブジェクトへアクセスするためのタブを作成します。さらにこれらのタブをアプリケーションとしてグループ化することによってアクセスしやすくします。最後にページレイアウトを調整し、講演者とセッションの関連が見えるようにします。

![](images/app.jpg)

## ステップ 1: タブの作成

セッションタブの作成を行います:

1. **設定** 画面より、**ビルド** > **作成** > **タブ** を選択します

1. **カスタムオブジェクトタブ** セクションの **新規** をクリックします。

1. オブジェクトに **セッション** を選択し、 タブスタイルの虫眼鏡アイコンより **本** アイコンを選択します。

1. **次へ**, **次へ** とクリックします。

1. **タブを含める** チェックボックスのチェックを外して、セッションタブが既存のどのアプリケーションにも表示されないことを確認し、**保存** をクリックします。(ステップ 2 で、セッションタブを新しいアプリケーションへ追加します)

講演者タブの作成を行います:

1. **設定** 画面より、**ビルド** > **作成** > **タブ** を選択します

1. オブジェクトに **講演者** を選択し、 タブスタイルの虫眼鏡アイコンより **本** アイコンを選択します。

1. **次へ**, **次へ** とクリックします。

1. **タブを含める** チェックボックスのチェックを外して、セッションタブが既存のどのアプリケーションにも表示されないことを確認し、**保存** をクリックします。

## ステップ 2: アプリケーションの作成

Salesforceのアプリケーションとはユーザが関連する機能に素早くアクセスできるようにするためにまとめられたタブのグループを指します。

1. **設定** 画面より、 **ビルド** > **作成** > **アプリケーション** を選択します。

1. **アプリケーション** セクションで **新規** をクリックします。

1. **カスタムアプリケーション** を選択し、 **次へ** をクリックします。

1. **カンファレンス管理** をアプリケーションの表示ラベルへ、 **Conference** をアプリケーション名に入力し、 **次へ** をクリックします。

1. デフォルトのアプリケーションロゴのまま **次へ** をクリックします。

1. **セッション** 及び **講演者** タブを **選択されたタブ** へ移動し、 **次へ** をクリックします。

1. **システム管理者** の **参照可能** チェックボックスにチェックを付け **保存** をクリックします。

    ![](images/sysadmin.jpg)

1. **カンファレンス管理** をアプリケーションセレクタより選択します (画面右上隅)

    ![](images/conference-app.jpg)

    > もしカンファレンス管理アプリケーショｎがアプリケーションセレクタに表示されない場合、おそらく設定画面でシステム管理者プロファイルへの権限付与を忘れています。設定画面より ビルド > 作成 > アプリケーション よりカンファレンス管理の横の編集をクリックして、システム管理者プロファイルの参照可能チェックを付け、保存をクリックします。

## ステップ 3: サンプルデータの入力

1. **講演者**　タブをクリックし、**新規** より、いくつかのサンプルの講演者を入力します。

1. **セッション** タブをクリックし、 **新規** ボタンより、幾つかサンプルのセッションを入力します。

1. 講演者をセッションへアサインします:
  - セッションの詳細画面より、**新規セッション_講演者** をクリックします。
  - 講演者の虫眼鏡アイコンより、講演者をポップアップダイアログより選択し、 **保存** をクリックします。

    ![](images/speaker-lookup.jpg)

    ![](images/session-detail.jpg)

    > ここで注意点として現時点での講演者リストや講演者ルックアップダイアログは、有益な情報を提供しない点に注意して下さい。これは次のステップにて修正します。



## ステップ 4: セッションページレイアウトを最適化する

In this step, you optimize the Session details screen: to allow the user to easily identify the speakers for a session, you add the appropriate fields to the Speaker list.  

![](images/session-layout.jpg)

1. In **Setup** mode, select **Build** > **Create** > **Objects**

1. Click the **Session** link

1. In the **Page Layouts** section, click **Edit** next to Session Layout

1. In the **Related Lists** section, click the wrench icon (Related list properties)

1. Add the following fields to the **Selected Fields**:
   - Speaker: Speaker Number
   - Speaker: First Name
   - Speaker: Last Name

1. Remove the following field from the **Selected Fields**:
  - Session Speaker: Session Speaker Name

1. Click **OK**

1. Click **Save** (upper left corner)

## Step 5: Optimize the Speaker Page Layout

In this step, you optimize the Speaker details screen: to allow the user to easily identify the sessions for a speaker, you add the appropriate fields to the Session list.  

![](images/speaker-layout.jpg)

1. In **Setup** mode, select **Build** > **Create** > **Objects**

1. Click the **Speaker** link

1. In the **Page Layouts** section, click **Edit** next to Speaker Layout

1. In the **Related Lists** section, click the wrench icon (Related list properties)

1. Add the following fields to the **Selected Fields**:
  - Session: Session Name
  - Session: Session Date

1. Remove the following field from the **Selected Fields**:
  - Session Speaker: Session Speaker Name

1. Click **OK**

1. Click **Save** (upper left corner)

## Step 6: Optimize the Speaker Lookup

In this step, you optimize the Speaker lookup dialog to allow the user to easily identify speakers.  

![](images/lookup.jpg)

1. In **Setup** mode, select **Build** > **Create** > **Objects**

1. Click the **Speaker** link

1. Scroll down to the **Search Layouts** section, and click **Edit** next to **Lookup Dialogs**

1. Add **First Name** and **Last Name** to the **Selected Fields**

1. Click **Save**

## ステップ 7: アプリケーションをテストする

1. Click the Sessions tab, select a session and make sure the speaker list shows the speaker number, first name, and last name

1. Assign a new speaker to a session and make sure the speaker lookup dialog shows the speaker first name and last name

1. Click the Speakers tab, select a speaker and make sure the session list shows the session name and date

> If the lists don't show the expected fields, you probably forgot to click the Save button in the Page Layout screen. Go back to steps 4 and 5, and make sure you click Save at the end.



<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Creating-the-Data-Model.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 戻る</a>
<a href="Creating-an-Apex-Class.html" class="btn btn-default pull-right">次へ <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
