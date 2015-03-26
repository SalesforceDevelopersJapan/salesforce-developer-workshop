---
layout: module
title: モジュール 8&#58; コントローラ拡張機能の作成
---

このモジュールでは、モジュール 7 で作成した Visualforce ページの機能を強化します。コントローラ拡張機能を作成して、スピーカーの写真をアップロードできるようにします

![](images/upload.jpg)

## ステップ 1: コントローラ拡張機能を作成する

このステップでは、シンプルなコントローラ拡張による **increment()** メソッド及び **counter** プロパティの
追加を通してコントローラ拡張の仕組みについて学びます。Increment ボタンをSpeakerForm ページ上で押下すると、 拡張コントローラのincrement() メソッドが、カウンタプロパティをインクリメントされ、新しい値が画面に自動的に表示されます。次のステップでは、講演者の写真のアップロードをサポートするなどより実用的な SpeakerControllerExtension を作成します。

1. 開発者コンソールで、 **File** > **New** > **ApexClass** の順にクリックし、クラス名に **SpeakerControllerExtension** と入力し、 **OK** をクリックします

1. 次のようにクラスを実装します:

    ```
    public class SpeakerControllerExtension {

        public Integer counter {get; set;}

        private final Speaker__c speaker;
        private ApexPages.StandardController stdController;

        public SpeakerControllerExtension(ApexPages.StandardController stdController) {
            this.speaker = (Speaker__c)stdController.getRecord();
            this.stdController = stdController;
            counter = 0;
        }

        public void increment() {
            counter++;
        }

    }
    ```
1. ファイルを保存します

1. 開発者コンソールでSpeakerFormのページを開き、ページの定義にコントローラ拡張機能を追加します:

    ```
    <apex:page standardController="Speaker__c" extensions="SpeakerControllerExtension">
    ```

1. Increment ボタンを追加します (保存ボタンのすぐ後):

    ```
    <apex:commandButton action="{!increment}" value="Increment"/>
    ```

1. counterを追加します (&lt;/apex:pageBlock>タグのすぐ後):

    ```
    {!counter}
    ```

1. ファイルを保存します

1. アプリケーションをテストします
  - Speakers タブをクリックし、speakerを選択し、 **編集** をクリックします
  - Incrementボタンを何回か押下し、ページ下部のカウンターの値が表示されることを確認します。


## Step 2: データモデルを拡張する

このステップでは、Speaker オブジェクトに 2 つの項目を追加します。1 つ目は[Picture_Path]で、サーバ上の画像の場所を格納します。2 つ目は **Picture** で、これは、Visualforce ページに画像を表示するために使われる数式項目です。

1. **設定** で **ビルド** > **作成** > **オブジェクト** の順に選択し、 **Speaker** リンクをクリックします

1. **カスタム項目＆リレーション** セクションで **新規** をクリックし、 **Picture_Path** を次のように定義します:
  - データ型：**テキスト**
  - 項目の表示ラベル：**Picture Path**
  - 文字数：**255**
  - 項目名：**Picture_Path**

  **次へ** 、 **次へ** 、 **保存＆新規** の順にクリックします

1. **Picture**項目を次のように定義します:
  - データ型：**数式**
  - 項目の表示ラベル：**Picture**
  - 項目名：**Picture**
  - 数式の戻り値のデータ型：**テキスト**
  - 数式: **IMAGE(Picture&#95;Path__c, '')**

        > 数式の指定では、二重引用符ではなく<strong>2つの単一引用符</strong>を使用してください。

  **次へ** 、 **次へ** 、 **保存** の順にクリックします。


## ステップ 3: 画像のアップロード機能を追加する

1. 開発者コンソールにて **SpeakerControllerExtension** を開きます。

1. counter変数の宣言、コンストラクタ内での初期化処理、そしてincrement()メソッドの宣言を削除します

1. 以下の様に変数を宣言します (　**speaker**変数宣言のすぐ前):

    ```
    public blob picture { get; set; }
    public String errorMessage { get; set; }
    ```

1. 標準コントローラのデフォルト挙動をオーバライドする形で、**save()** メソッドを以下の様に実装します (コンストラクタメソッドのすぐ後):

    ```
    public PageReference save() {
        errorMessage = '';
        try {
            upsert speaker;
            if (picture != null) {
                Attachment attachment = new Attachment();
                attachment.body = picture;
                attachment.name = 'speaker_' + speaker.id + '.jpg';
                attachment.parentid = speaker.id;
                attachment.ContentType = 'application/jpg';
                insert attachment;
                speaker.Picture_Path__c = '/servlet/servlet.FileDownload?file='
                                          + attachment.id;
                update speaker;
            }
            return new ApexPages.StandardController(speaker).view();
        } catch(System.Exception ex) {
            errorMessage = ex.getMessage();
            return null;
        }
    }
    ```

1. ファイルを保存します

1. 開発者コンソールでSpeakerFormのページを開ます

1. Remove the Increment button

1. inputFileを追加します（EmailのinputFieldの直後）:

    ```
    <apex:inputFile value="{!picture}" accept="image/*" />
    ```

1. カウンターの代わりに、 &lt;/apex:pageBlock>の直後にエラーメッセージを表示するようにします 。

    ```
    {!errorMessage}
    ```

1. ファイルを保存します。

## ステップ 4: アプリケーションをテストする

1. 講演者タブをクリックします

1. 新規をクリックして講演者レコードを作成します

1. スピーカーの名、姓、メールアドレスを入力します

1. **参照** ボタンをクリックしてファイルシステム上のjpgファイルを選択します

1. **保存** ボタンをクリックして、レコードの詳細ページに写真が表示されることを確認します。


<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Creating-a-Visualforce-Page.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 戻る</a>
<a href="Using-JavaScript-in-Visualforce-Pages.html" class="btn btn-default pull-right">次へ <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
