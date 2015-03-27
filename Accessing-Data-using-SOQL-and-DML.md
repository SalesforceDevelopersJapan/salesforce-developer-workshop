---
layout: module
title: モジュール 5&#58; SOQL と DML を使用したデータへのアクセス
---
**SOQL** とは、 **Salesforce Object Query Language** の略です。これは SQL に似た言語で、Salesforce 内でのデータの取得に使われます。また、**DML** とは、 **Data Manipulation Language** の略で、Salesforce の DML は、データの挿入、更新、削除に使われます。このモジュールでは、SOQL と DML を知っていただくため、開発者コンソールでまず使ってみます。次のモジュールでは、Apex クラスや Apex トリガで、これらを実際に使っていきます。

## ステップ 1: SOQL ステートメントを実行する

1. 開発者コンソールで、ウィンドウの下部にある **Query Editor** タブをクリックします。

    ![](images/queryeditor.jpg)

1. 次のSOQLステートメントを入力し、**Execute** ボタンをクリックすると、カンファレンスのセッションを取得できます:

    ```
    SELECT Id, Name, Session_Date__c, Level__c FROM Session__c
    ```

1. 次のステートメントを入力し、**Execute** ボタンをクリックすると、Levelが **Beginner** 向けのセッションを取得できます(前のステップで「Beginner」レベルのレコードを作成していることが前提):  

    ```
    SELECT Id, Name, Session_Date__c, Level__c FROM Session__c
        WHERE Level__c = 'Beginner'
    ```

1. 次のステートメントを入力し、 **Execute** ボタンをクリックすると、スピーカーの名前順のリストを取得できます:

    ```
    SELECT Id, First_Name__c, Last_Name__c, Email__c FROM Speaker__c
        ORDER BY First_Name__c, Last_Name__c
    ```

1. 次のステートメントを入力し、 **Execute** ボタンをクリックすると、スピーカーに割り当てられたセッションのリストを、セッション、スピーカーの情報と合わせて取得できます:

    ```
    SELECT Session__r.Name,
           Session__r.Session_Date__c,
           Speaker__r.First_Name__c,
           Speaker__r.Last_Name__c
        FROM Session_Speaker__c
        ORDER BY Session__r.Session_Date__c, Session__r.Name
    ```


## ステップ 2: DML ステートメントを実行する


1. 開発者コンソールで、**Debug** > **Open Execute Anonymous Window** の順にクリックし、次のステートメントを実
行してセッションを作成します。:

    ```
    Session__c session=new Session__c(Name='Advanced Apex', Level__c='Advanced');
    insert session;
    ```

    Query Editor を使い、ステップ 1 の説明に従って SOQL ステートメントを実行すると、上記セッションが作成されたことを確認できます。


2. セッションの内容を更新するには、次のステートメントを実行します:

    ```
    Session__c session = [SELECT Id FROM Session__c WHERE NAME='Advanced Apex'];
    session.Level__c = 'Intermediate';
    update session;
    ```

    先ほどと同様、Query Editor で SOQL ステートメントを実行すると、セッションが適切に更新されたことを確認できます。


## 関連リソース

- [SOQL 及び SOSL に関するリファレンス](https://developer.salesforce.com/docs/atlas.ja-jp.soql_sosl.meta/soql_sosl/)


<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Creating-an-Apex-Class.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> 戻る</a>
<a href="Creating-Triggers.html" class="btn btn-default pull-right">次へ <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
