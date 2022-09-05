---
title: D365FOでダウンタイムなしでカスタム X++ スクリプトを実行する方法
date: 2022-08-30
tags:
  - D365FO
  - Data fix

disableDisclaimer: false
---

こんにちは、日本マイクロソフトの細野です。
この記事では、Dynamics 365 Finance and Operationsにて、ダウンタイムなしでカスタム X++ スクリプトを実行する方法を紹介します。

<!-- more -->
## 検証に用いた製品・バージョン:
Dynamics 365 Finance and Operations
Application version: 10.0.28
Platform version: PU52

## 手順
1. クラウドホスト環境、開発環境等にて Visual Studio を開く
2. Dynamics 365 > Model Management > Create model を開く
    ![](./how-to-run-custom-script-d365fo/step2.png)
3.  Model name、Model publisher、Model descripstion を入力し "Next" をクリックする
    ![](./how-to-run-custom-script-d365fo/step3.png)
4.  "Create new package" を選択し "Next" をクリックする
    ![](./how-to-run-custom-script-d365fo/step4.png)
5. ApplicationPlatform、ApplicationSuite、SourceDocumentationTypes の3つを選択し "Next" をクリックする
    ![](./how-to-run-custom-script-d365fo/step5-1.png)
    ![](./how-to-run-custom-script-d365fo/step5-2.png)

    ※ スクリプトの内容により選択するモデルが異なる場合がございます。

6. 入力、選択した内容を確認し "Next" をクリックする
    ![](./how-to-run-custom-script-d365fo/step6.png)
7. Project name にて名前を入力し "Create" を選択する
    ![](./how-to-run-custom-script-d365fo/step7.png)
8. Project の作成完了後、Solution Explorer にて右クリック Add > New Item を選択し ”Add New Item” のダイアログを開く
    ![](./how-to-run-custom-script-d365fo/step8.png)
9.  FinanceOperations > Dynamics 365 Items を開き、Code を選択、Runnable Class(Job) を選択する
    ![](./how-to-run-custom-script-d365fo/step9.png)
10. 作成された Class に修正スクリプトを記載する
    ![](./how-to-run-custom-script-d365fo/step10-1.png)
例. 
    ``` c++
    class FixSO000811
    {
        public static void main(Args _args)
         {
            ttsbegin;

            InventTrans inventTrans;

            select forupdate inventTrans
                where inventTrans.RecId == 68719841375;

            inventTrans.Qty = -10;
            inventTrans.Update();

            ttscommit;

            Info('Script completed successfully');
        }
    }
    ```
    ![](./how-to-run-custom-script-d365fo/step10-2.png)
    
11.	Dynamics 365 > Deploy > Create Deployment Package を開く
    ![](./how-to-run-custom-script-d365fo/step11.png)
12.	対象の Class を選択、保存先のロケーションを入力し "Create" を選択する
    ![](./how-to-run-custom-script-d365fo/step12-1.png)
    ![](./how-to-run-custom-script-d365fo/step12-2.png)
    ![](./how-to-run-custom-script-d365fo/step12-3.png)
13.	Dynamics 365 Finance and Operations を開き、システム管理 > 定期処理 > データベース > カスタムスクリプト に移動する
    ![](./how-to-run-custom-script-d365fo/step13.png)
14.	カスタムスクリプトにてアップロードを選択し、作成したパッケージをアップロードする
    ![](./how-to-run-custom-script-d365fo/step14-1.png)
    ![](./how-to-run-custom-script-d365fo/step14-2.png)
15.	スクリプトの承認のため、別ユーザーで同環境にログインする
    スクリプトを誤って実行することがないように、アップロードを実行したユーザーでは承認できないように制御されています。必ずほかのユーザーで承認を実行する必要があります。
16.	レコードを選択し、詳細画面を開き「承認」をクリックする
    ![](./how-to-run-custom-script-d365fo/step16-1.png)
    ![](./how-to-run-custom-script-d365fo/step16-2.png)
    ![](./how-to-run-custom-script-d365fo/step16-3.png)
17.	「テストの実行」を選択する
    ![](./how-to-run-custom-script-d365fo/step17.png)
18. ログの内容を確認し、問題なければ「テスト ログを受け入れる」を選択する
    ![](./how-to-run-custom-script-d365fo/step18-1.png)
    ![](./how-to-run-custom-script-d365fo/step18-2.png)
19. 「実行」を選択する
    ![](./how-to-run-custom-script-d365fo/step19-1.png)
    ![](./how-to-run-custom-script-d365fo/step19-2.png)
20. 実行後のログを確認する
    ![](./how-to-run-custom-script-d365fo/step20.png)
21. 対象のレコードが想定通りに修正されていることを確認する
    ![](./how-to-run-custom-script-d365fo/step21.png)
22. 「解決済みの目的」をクリックし、スクリプトの状態を解決済みに更新する
    ![](./how-to-run-custom-script-d365fo/step22.png)

## 注意
上記の手順、手順内の画像については本記事の執筆時のものです。
実際の画面とは挙動に違いがある可能性がございます。

手順内に例示のスクリプトは、デモデータに対して実行可能なものとなりますが実行結果を保証するものではありません。実行する際には検証環境等で影響範囲を十分に検証のうえ、実施いただきますようお願いいたします。

## 参考
（関連公開資料）
https://docs.microsoft.com/ja-jp/dynamics365/fin-ops-core/dev-itpro/deployment/create-apply-deployable-package
https://docs.microsoft.com/ja-jp/dynamics365/fin-ops-core/dev-itpro/deployment/run-custom-scripts

---
## おわりに  

以上、Dynamics 365 Finance and Operations にて、ダウンタイムなしでカスタム X++ スクリプトを実行する方法を紹介いたしました。もし、お困りのこと等がございましたら、弊社までお問い合わせ頂きますようお願いいたします。
