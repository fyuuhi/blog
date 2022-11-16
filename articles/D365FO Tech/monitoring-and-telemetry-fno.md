---
title: Monitoring and Telemetry 機能を利用してD365FOのログをApplication Insightsで確認する方法
date: 2022-11-08
tags:
  - D365FO
  - Tech
  - Information
disableDisclaimer: false
---

こんにちは、Dynamics ERP サポートチームの福原です。

この記事では、Monitoring and Telemetry 機能を利用してDynamics 365 for Finance and Operations (D365FO) のログをApplication Insights で確認する方法についてご案内します。
D365FO で発生したイベント、エラーの情報等をApplication Insights に送信することで、Azure portal 上で作成したお客様が所持しているApplication Insights 上でログの確認を行うことができます。

<!-- more -->
---
https://learn.microsoft.com/en-us/dynamics365/fin-ops-core/dev-itpro/sysadmin/monitoring-and-telemetry-appinsights

まずはMonitoring and Telemetry 機能を利用する手順についてご案内します。
Monitoring and Telemetry 機能はバージョン10.0.30 より利用可能なプレビュー機能となり、既定では無効化されていますので、機能管理ページにてMonitoring and Telemetry 機能を有効化する必要があります。
まず、システム管理 > 機能管理 ワークスペースの"すべて"タブにて、monitoring と検索しますと、Monotoring and Telemetry 機能が表示されますので、今すぐ有効化をクリックします。
プレビュー機能となりますので、注意事項が表示されてますので、ご確認頂き、同意いただける場合には先に進んでください。
    <img src="./image1.png" style="border: 1px black solid;">

機能を有効化しましたら、D365FO のUI にて、システム管理 > 設定 > Monitoring and telemetry parameters ページにアクセスできるようになります。
アクセスしますと、Configure 、Environments 、Application Insights Registry タブが表示されます。
Configure タブでは、Application Insights に記録したいログの種類を有効化します。
    <img src="./image2.png" style="border: 1px black solid;">

Environments タブでは、以下の情報を入力します。
  - LCS environment ID: LCS の環境の詳細ページに記載の環境ID
  - Environment Mode: クラウドホスト環境の場合はDevelopment 、サンドボックス環境の場合はTest 、本番環境の場合はProduction 
      <img src="./image3.png" style="border: 1px black solid;">

Application Insights Registry タブでは、以下の情報を入力します。
  - Environment Mode: クラウドホスト環境の場合はDevelopment 、サンドボックス環境の場合はTest 、本番環境の場合はProduction 
  - Instrumentation key: Application Insights 作成しますとインストルメンテーション キー が発行されます (https://learn.microsoft.com/ja-jp/azure/azure-monitor/app/create-new-resource#copy-the-instrumentation-key).
    <img src="./image4.png" style="border: 1px black solid;">

    <img src="./image5.png" style="border: 1px black solid;">

上記の設定が完了しましたら、多少のタイムラグはございますが、D365FO 上のフォームの操作、バッチジョブのエラー等がApplication Insights に記録されるようになります。

例と致しまして、以下ではデータ管理のエクスポートバッチジョブが失敗した場合に、D365FO のUI 上で表示されている"Export execution failed for entity Batch jobs history: Export failed while copying data from staging to target: 'エクスポート プロセス: test11081'" というエラーメッセージが、自動でApplication Insights にも連携されています。
    <img src="./image7.png" style="border: 1px black solid;">
    <img src="./image6.png" style="border: 1px black solid;">



---
## おわりに  
以上、Monitoring and Telemetry 機能を利用してDynamics 365 for Finance and Operations (D365FO) のログをApplication Insights で確認する方法について紹介いたしました。
より詳細な情報が必要な場合、弊社テクニカルサポートまでお問い合わせください。
