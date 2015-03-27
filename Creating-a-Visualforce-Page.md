---
layout: module
title: モジュール 7&#58; Visualforceページの作成
---
このモジュールでは、Speakerレコードの新規作成や編集を行うためのカスタムユーザインターフェースとなるVisualforceページを作成します。

## ステップ 1: Visualforceページ **SpeakerForm** を作成する

1. 開発者コンソールで **File** > **New** > **Visualforce Page** の順にクリックし、ページ名に **SpeakerForm** と入力し、 **OK** をクリックします。

1. 2.	次のように入力し、SpeakerFormを実装します:

    ```
    <apex:page standardController="Speaker__c">
    <apex:form >
        <apex:pageBlock title="講演者の編集">
            <apex:pageBlockSection columns="1">
                <apex:inputField value="{!Speaker__c.Last_Name__c}"/>
                <apex:inputField value="{!Speaker__c.First_Name__c}"/>
                <apex:inputField value="{!Speaker__c.Email__c}"/>
            </apex:pageBlockSection>
            <apex:pageBlockButtons >
                <apex:commandButton action="{!save}" value="保存"/>
            </apex:pageBlockButtons>
        </apex:pageBlock>
    </apex:form>
    </apex:page>
    ```

1. ファイルを保存します。

1. 作成したVisualforceページをテストします。複数のテスト方法がありますが、たとえば、次のような手順で確認が可能です:
  - 開発者コンソールで、SpeakerForm のエディタ画面を開き、**Preview** ボタンをクリックします(画面の 左上)
  - ブラウザで、インスタンスのドメイン名に/apex/SpeakerForm を追加し、Visualforce ページに直接アク セスします。たとえば、「[https://na17.salesforce.com/apex/SpeakerForm](https://na17.salesforce.com/apex/SpeakerForm)」のように指定します (「/apex/SpeakerForm」の前に、自分の Salesforce 組織のドメイン名を指定)


## ステップ 2: SpeakerForm をデフォルトのフォームに設定する

このステップでは、**SpeakerForm**ページを、講演者レコードの新規作成や編集に使用するデフォルトのフォームに設定します。:

1. **設定** メニューで **ビルド** > **作成** > **オブジェクト** の順に選択し、 **講演者** リンクをクリックします

1. 下へスクロールして **ボタン、リンク、およびアクション** まで移動し、表示ラベル **新規** の左にある **編集** をクリックします

1. **上書き手段** で **Visualforceページ** を選択し、**SpeakerForm** を選択します。

1. **保存** をクリックします

1. **ボタン、リンク、およびアクション** セクションで、表示ラベル **編集** の左にある **編集** をクリックします

1. **上書き手段** で **Visualforceページ** を選択し、 **SpeakerForm** を選択します。

1. **保存** をクリックします

## ステップ 3: アプリケーションをテストする

1. **講演者**タブをクリックします

1. **新規** をクリックして、自分が作成したVisualforceページが表示されることを確認します

1. 新しい講演者レコードを作成して **保存** を選択します。

> この段階では、作成したVisualforceページには、元のSpeakerレコード用フォームを超える機能はありません。次のモジュールで、スピーカーの写真のアップロードをサポートできるよう、SpeakerFormを拡張します。




<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Creating-Triggers.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 戻る</a>
<a href="Creating-a-Controller-Extension.html" class="btn btn-default pull-right">次へ <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
