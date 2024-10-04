# Notify-MDC-CWP-MailTeams
このレポジトリは、MDC CWP アラートを他テナントに Azure Communication Eメールサービス / Incoming Webhook を用いて通知するためのテンプレートを提供しています。

# 構成イメージ
このレポジトリはロジックアプリのテンプレートを提供します。<BR>
構成は Entra ID 他テナントに対して、電子メール (Azure Communication Service 経由) / Microsoft Teams (Incoming Webhook) を用いた通知を想定しています。

![image](https://github.com/user-attachments/assets/72694942-5d88-4d62-8d13-d02a30b0cac4)

# Deploy To Azure
> 以下ボタンより、ARMテンプレートを展開して導入して下さい
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)]https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhisashin0728%2FNotify-MDC-CWP-MailTeams%2Frefs%2Fheads%2Fmain%2FMdc-Cwpp-AOAIAlert.json)

- 

# 必要なサービス
本構成を実現するために、以下 Azure リソースが必要となります。
- 通信サービス (Azure Communication Service) / 電子メールサービス
  - Azure 基盤から外部メール送信するために必要
  - Azure Communication Service に接続するための「接続文字列」を準備してください
- Azure OpenAI サービス
  - サブスクリプションに Azure OpenAI をデプロイ & モデルデプロイまで完了して下さい
  - Azure OpenAI のエンドポイント (接続先URL)、モデルデプロイ名（GPT4など)を準備して下さい
