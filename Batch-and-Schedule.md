---
layout: module
title: モジュール 12&#58; 一括処理とスケジュール設定
---
このモジュールでは、カンファレンスのスピーカーにリマインダーメールを送信するための一括処理を作成して、実行します。

## ステップ 1: EmailManagerクラスを作成する

1. 開発者コンソールで、**File** > **New** > **Apex Class** の順にクリックします。クラス名に **SendReminderEmail** と入力し、 **OK** をクリックします。

1. クラスをglobalに変更してBatchableインターフェースを実装し、そのインターフェースで3つのメソッドを定義します。:

    ```
    global class SendReminderEmail implements Database.Batchable<sObject> {

        global Database.QueryLocator start(Database.BatchableContext bc) {

        }

        global void execute(Database.BatchableContext bc, List<Speaker__c> scope) {

        }

        global void finish(Database.BatchableContext bc) {

        }

    }
    ```

1. 3つのインスタンス変数を宣言します（それぞれ、クエリ、メールの件名、メールの本文を格納）:

    ```
    global String query;
    global String subject;
    global String body;
    ```

1. **コンストラクタ** を次のように定義します:

    ```
    global SendReminderEmail(String query, String subject, String body) {
        this.query = query;
        this.subject = subject;
        this.body = body;
    }
    ```

1. **start()** メソッドを次のように実装します:

    ```
    global Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator(query);
    }
    ```

1. **execute()** メソッドを次のように実装します:

    ```
    global void execute(Database.BatchableContext bc, List<Speaker__c> scope) {
        for (Speaker__c speaker : scope) {
            EmailManager.sendMail(speaker.Email__c, this.subject, this.body);
        }
    }
    ```

1. 7.	ファイルを保存します


## ステップ 2: 一括処理を実行する

1. 講演者レコードのいずれか1件に、自分のメールアドレスを割り当てておきます

1. 開発者コンソールで、 **Debug** > **Open Execute Anonymous Window** の順にクリックします

1. 次のApexコードを入力します:

    ```
    String q = 'SELECT First_Name__c, Last_Name__c, Email__c FROM Speaker__c WHERE Email__c != null';
    SendReminderEmail batch = new SendReminderEmail(q, '最終確認', 'カンファレンスは来週月曜から開催です!!');
    Database.executeBatch(batch);
    ```

1. **Execute** をクリックします

1. メールを確認します


<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Testing.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 戻る</a>
<a href="next.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
