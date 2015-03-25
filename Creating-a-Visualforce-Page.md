---
layout: module
title: Module 7&#58; Creating a Visualforce Page
---
In this module, you create a Visualforce page to provide a custom user interface for creating and editing speakers.

## Step 1: Create the SpeakerForm Visualforce Page

1. In the **Developer Console**, select **File** > **New** > **Visualforce Page**, specify **SpeakerForm** as the page name and click **OK**

1. ステップ 1:Visualforce ページ「SpeakerForm」を作成する:

    ```
    <apex:page standardController="Speaker__c">
    <apex:form >
        <apex:pageBlock title="Edit Speaker">
            <apex:pageBlockSection columns="1">
                <apex:inputField value="{!Speaker__c.First_Name__c}"/>
                <apex:inputField value="{!Speaker__c.Last_Name__c}"/>
                <apex:inputField value="{!Speaker__c.Email__c}"/>
            </apex:pageBlockSection>
            <apex:pageBlockButtons >
                <apex:commandButton action="{!save}" value="Save"/>
            </apex:pageBlockButtons>
        </apex:pageBlock>
    </apex:form>
    </apex:page>
    ```

1. ファイルを保存します。

1. 作成したVisualforceページをテストします。複数のテスト方法がありますが、たとえば、次のような手順で確認が可能です:
  - 開発者コンソールで、SpeakerForm のエディタ画面を開き、**Preview** ボタンをクリックします(画面の 左上)
  - ブラウザで、インスタンスのドメイン名に/apex/SpeakerForm を追加し、Visualforce ページに直接アク セスします。たとえば、「[https://na17.salesforce.com/apex/SpeakerForm](https://na17.salesforce.com/apex/SpeakerForm)」のように指定します (「/apex/SpeakerForm」の前に、自分の Salesforce 組織のドメイン名を指定)


## ステップ 2:SpeakerForm をデフォルトのフォームに設定する

このステップでは、**SpeakerForm**ページを、Speaker レコードの新規作成や編集に使用するデフォルトのフォームに設定します。:

1. In **Setup**, select **Build** > **Create** > **Objects** and click the **Speaker** link

1. Scroll down to the **Buttons, Links, and Actions** section, and click **Edit** next to **New**

1. Check Override With Visualforce Page, and select **SpeakerForm**

1. Click **Save**

1. In the **Buttons, Links, and Actions** section, click **Edit** next to **Edit**

1. In **Override With** select **Visualforce Page** and select **SpeakerForm**

1. Click **Save**

## Step 3: Test the Application

1. Click the **Speakers** Tab

2. Click **New** which should display your Visualforce page

3. Create a new Speaker and select **Save**

> At this stage, the Visualforce page doesn't provide any additional capability compared to the default speaker form. In
the next module, you will enhance SpeakerForm to support the upload of the speaker's pictures.



<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Creating-Triggers.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> Previous</a>
<a href="Creating-a-Controller-Extension.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
