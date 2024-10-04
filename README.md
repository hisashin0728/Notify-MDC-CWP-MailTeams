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
- Microsoft Teams / Incoming Webhook の設定
  -　[Microsoft Teamsのワークフローを使用して受信 Webhook を作成する](https://support.microsoft.com/ja-jp/office/create-incoming-webhooks-with-workflows-for-microsoft-teams-8ae491c7-0394-4861-ba59-055e33f75498#:~:text=%E5%8F%97%E4%BF%A1%20Webhook%20%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B%E3%81%A8%E3%80%81%E5%A4%96%E9%83%A8%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AFMicrosoft%20Teams%E5%86%85%E3%81%AE%E3%83%81%E3%83%A3%E3%83%83%E3%83%88%E3%81%A8%E3%83%81%E3%83%A3%E3%83%8D%E3%83%AB%E3%81%AE%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E3%82%92%E5%85%B1%E6%9C%89%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82%20Webhook%20%E3%81%AF%E3%80%81%E8%BF%BD%E8%B7%A1%E3%81%A8%E9%80%9A%E7%9F%A5%E3%82%92%E8%A1%8C%E3%81%86%E3%83%84%E3%83%BC%E3%83%AB%E3%81%A8%E3%81%97%E3%81%A6%E4%BD%BF%E7%94%A8%E3%81%95%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82%20Webhook%20%E8%A6%81%E6%B1%82%E3%81%8C%E5%8F%97%E4%BF%A1%E3%81%95%E3%82%8C%E3%81%9F%E3%81%A8%E3%81%8D%E3%81%AB%E3%80%81%E3%83%81%E3%83%A3%E3%83%8D%E3%83%AB%E3%81%AB%E6%8A%95%E7%A8%BF%E3%81%97%E3%81%9F%E3%82%8A%E3%83%81%E3%83%A3%E3%83%83%E3%83%88%E3%81%97%E3%81%9F%E3%82%8A%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82,%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%82%92%E9%81%B8%E6%8A%9E%E3%81%97%E3%81%BE%E3%81%99%E3%80%82%20%E5%90%84%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%AB%E3%81%AF%E3%80%81%E7%95%B0%E3%81%AA%E3%82%8B%E8%AA%8D%E8%A8%BC%E3%81%AE%E7%A8%AE%E9%A1%9E%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82%20%E3%83%81%E3%83%A3%E3%83%83%E3%83%88%20Webhook%20%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%20%E3%83%81%E3%83%A3%E3%83%8D%E3%83%AB%20Webhook%20%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88)

