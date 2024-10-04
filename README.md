# Notify-MDC-CWP-MailTeams
このレポジトリは、MDC CWP アラートを他テナントに Azure Communication Eメールサービス / Incoming Webhook を用いて通知するためのテンプレートを提供しています。

# 1. 構成イメージ
このレポジトリはロジックアプリのテンプレートを提供します。<BR>
構成は Entra ID 他テナントに対して、電子メール (Azure Communication Service 経由) / Microsoft Teams (Incoming Webhook) を用いた通知を想定しています。

![image](https://github.com/user-attachments/assets/72694942-5d88-4d62-8d13-d02a30b0cac4)

# 2. Deploy To Azure
> 以下ボタンより、ARMテンプレートを展開して導入して下さい

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhisashin0728%2FNotify-MDC-CWP-MailTeams%2Frefs%2Fheads%2Fmain%2FMdc-Cwpp-AOAIAlert.json)

- ロジックアプリを展開するサブスクリプション / リソースグループを指定
- リージョンはお客様の環境に合わせて設定
- プレイブック名は任意です (デフォルトは ``Notify-MDC-CWP-MailTeams``)
- Send To Email Address は、通知する先の Email アドレスを設定して下さい

![image](https://github.com/user-attachments/assets/5b602c99-70d8-4c23-a546-11c89ef3d605)

# 3. デプロイ後の設定作業 (サブスクリプションの所有者権限が必要）
> デプロイ後、以下設定を行って下さい。

- ロジックアプリのマネージド ID に OpenAI ユーザーの RBAC 権限を付与
  - 展開されたロジックアプリの「ID」を選択

![image](https://github.com/user-attachments/assets/dd0d4591-e30d-4080-acfb-5ceb503d4ab1)

  - 「Azure ロールの割り当て」から、Azure OpenAI サービスがあるサブスクリプションに対して、「Azure OpenAI ユーザー」権限を付与

![image](https://github.com/user-attachments/assets/78660148-0648-4a19-8d7c-90b598a33d30)

# 4. デプロイ後のチューニング作業 (ロジックアプリコントリビューターロールで実施可能）
> 以下は本構成を行う際のチューニングになります。

- 「API 接続」より、「API接続の編集」から通信サービスの接続文字列を追加
- ロジックアプリのコードビューから以下をお客様環境用に修正
  - Azure OpenAI エンドポイントの修正
  - Teams 通知用 Incoming Webhook リンクの修正

# [補足] 必要なサービス
本構成を実現するために、以下 Azure リソースが必要となります。
- 通信サービス (Azure Communication Service) / 電子メールサービス
  - Azure 基盤から外部メール送信するために必要
  - Azure Communication Service に接続するための「接続文字列」を準備してください
- Azure OpenAI サービス
  - サブスクリプションに Azure OpenAI をデプロイ & モデルデプロイまで完了して下さい
  - Azure OpenAI のエンドポイント (接続先URL)、モデルデプロイ名（GPT4など)を準備して下さい
