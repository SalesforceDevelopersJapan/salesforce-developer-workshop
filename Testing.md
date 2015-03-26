---
layout: module
title: Module 11&#58; ユニットテスト
---
このモジュールでは、モジュール6で作成したダブルブッキングを回避するRejectDoubleBookingトリガのテストを実行します。

## ステップ 1: テストクラスを作成する

1. 開発者コンソールで、 **File** > **New** > **Apex Class** の順にクリックし、クラス名に **TestRejectDoubleBooking** と入力し、 **OK** をクリックします。

1. クラスをpublicから **private** に変更し、**@isTest** というアノテーションを追加します:

    ```
    @isTest
    private class TestRejectDoubleBooking{

    }
    ```

## ステップ 2: 通常の予約へのテストを実行するテストメソッドを追加する

1. TestRejectDoubleBookingクラスにTestSingleBooking()メソッドを追加します。これにより、RejectDoubleBookingトリガが、通常の予約（ダブルブッキングが起きていない予約）を妨げるものでないことを確認します。:

    ```
    static testmethod void TestSingleBooking() {
        Datetime now = System.now();

        Speaker__c speaker = new Speaker__c(First_Name__c='John', Last_Name__c='Smith');
        insert speaker;

        Session__c session = new Session__c(Name='Some Session', Session_Date__c=now);
        insert session;

        Session_Speaker__c assignment =
            new Session_Speaker__c(Session__c=session.Id, Speaker__c=speaker.Id);
        Test.startTest();
        Database.SaveResult result = Database.insert(assignment, false);
        Test.stopTest();

        System.assert(result.isSuccess());
    }
    ```

1. ファイルを保存します

1. ウィンドウの右上にある［Run Test］をクリックします

1. ウィンドウの下部にある **Tests** タブをクリックし、テスト結果を確認します

    ![](images/test1.jpg)


## ステップ 3: ダブルブッキングをテストするメソッドを追加する

1. TestRejectDoubleBookingクラスにTestDoubleBooking()メソッドを追加します。これにより、RejectDoubleBookingトリガが、実際にダブルブッキングを却下することを確認します:

    ```
    static testmethod void TestDoubleBooking() {
        Datetime now = System.now();

        Speaker__c speaker = new Speaker__c(First_Name__c='John', Last_Name__c='Smith');
        insert speaker;

        Session__c session1 = new Session__c(Name='Session 1', Session_Date__c=now);
        insert session1;
        Session__c session2 = new Session__c(Name='Session 2', Session_Date__c=now);
        insert session2;

        Session_Speaker__c assignment1 =
            new Session_Speaker__c(Session__c=session1.Id, Speaker__c=speaker.Id);
        insert assignment1;

        Session_Speaker__c assignment2 =
            new Session_Speaker__c(Session__c=session2.Id, Speaker__c=speaker.Id);
        Test.startTest();
        Database.SaveResult result = Database.insert(assignment2, false);
        Test.stopTest();

        System.assert(!result.isSuccess());
    }
    ```

1. ファイルを保存します

1. ウィンドウの右上にある **Run Test** をクリックします

1. ウィンドウの下部にある **Tests** タブをクリックし、テスト結果を確認します

    ![](images/test2.jpg)



<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Using-the-Salesforce1-Platform-APIs.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 戻る</a>
<a href="Batch-and-Schedule.html" class="btn btn-default pull-right">次へ <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
