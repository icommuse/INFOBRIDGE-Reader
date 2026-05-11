# INFOBRIDGE Reader
**INFOBRIDGEエコシステム向けセキュア・デコーダー**

[![Open Live Preview](https://img.shields.io/badge/GitHub%20Pages-Open%20Live%20Preview-blue?style=for-the-badge&logo=github)](https://icommuse.github.io/INFOBRIDGE-Reader/index.html)
[![Download Latest ZIP](https://img.shields.io/badge/Release-Download%20Offline%20ZIP-green?style=for-the-badge&logo=archive)](https://github.com/icommuse/INFOBRIDGE-Reader/releases)

## 🌐 INFOBRIDGE Ecosystem

1. **カプセル化（iOSアプリ）**: データを**安全な英数字パケット**（Base64またはAES-256暗号化）へ変換
2. **伝送（QR Wedge）**: スキャナが英数字列をキーボード入力として送信
3. **復元（Reader）**: Readerがパケットを再構成し、元の内容を復元またはローカル復号

---

## 📖 INFOBRIDGE Reader について

INFOBRIDGE Readerは、INFOBRIDGE iOSアプリからQRコードで送信される分割データを再構成・復元するために設計された、オフラインファーストのWebユーティリティです。エアギャップ環境における安全なデータ移送を実現します。

> [!IMPORTANT]
> **ガバナンスとコンプライアンス**
> 医療機関、官公庁、企業の高セキュリティ環境で利用する場合は、必ず所属組織の情報セキュリティポリシーに従ってください。多くのケースで、導入・検証・端末設定には**IT管理者**の関与が必要です。

---

## 🚀 すぐに試す（PWA / ライブプレビュー）
公式GitHub Pages URLから、すぐにReaderを試すか、**PWA（Progressive Web App）**としてインストールできます。
👉 **[https://icommuse.github.io/INFOBRIDGE-Reader/index.html](https://icommuse.github.io/INFOBRIDGE-Reader/index.html)**

> [!TIP]
> このURL経由でアクセスすると、Chrome/Edgeの**Install App**やSafari(Mac)の**Dockに追加**が利用できます。PWAで独立ウィンドウ実行にするとURLバーが非表示になり、スキャナ入力の漏れを防ぎやすくなります。

---

## 📦 業務導入とオフライン運用
INFOBRIDGE Readerはオフラインファーストですが、最新ブラウザの安全機構を活用するため**PWA**として構築されています。独立したアプリとしてインストールすることでURLバーが除去され、ブラウザの入力履歴にスキャナ内容が残るのを防ぐことができます。

> [!CAUTION]
> **プロトコル制限**
> `file://`で`index.html`を直接開く方法は、業務利用では**推奨しません**。ブラウザ仕様により、fileプロトコルではService WorkerとPWAインストールが無効化され、スキャナ入力がURLバーへ漏れるリスクが高まります。

### エアギャップ環境での導入方式

高セキュリティ端末では、次のいずれかの方法で運用してください。

#### A. 組織内イントラネットサーバー（推奨）
ZIPの内容を**内部Webサーバー**または**組織イントラネット**で配信する方式が最も堅牢です。
- **要件**: `https://`（または信頼済みイントラネットに限定した`http://`）で提供
- **利点**: ネイティブに近いPWAとして導入可能

#### B. ローカルSecure Context（端末単位）
中央サーバーがない場合は、端末上にローカルのSecure Contextを構築します。
1. **ダウンロード**: [Latest Releases](https://github.com/icommuse/INFOBRIDGE-Reader/releases/latest)から配布物を取得し展開
2. **ローカルホスト起動**: Python等のランタイムでローカル配信（例: `python3 -m http.server 8080`）
3. **アクセス**: `http://localhost:8080/index.html` を開く
4. **仕上げ**: ブラウザの**Install App**で独立ウィンドウ化（URLバー非表示）

詳細手順は [README.txt](./README.txt) を参照してください。

---

## 🛰️ セキュリティと技術的信頼性

INFOBRIDGE Readerは、医療・法務・行政などの高セキュリティ用途を想定し、透明性と検証可能性を重視しています。

- **AES-256-GCM標準暗号**: Secretモードのパケットはブラウザのローカルメモリ内で復号され、アップロード・永続保存されません。
- **エアギャップ前提のデータ搬送**: QRコードという光学伝送を使うことで、ネットワーク起因の攻撃面を縮小します。
- **External API Isolation**: 再構成と暗号処理はブラウザ内で完結し、デコードパイプラインは外部APIを呼び出しません。
- **検証可能なプロトコル**: Base64と標準的な暗号方式を用いており、外部監査に適しています。
- **Zero-Installation Footprint**: ドライバ不要のポータブルWebツールとして動作し、攻撃面を抑制します。

---

## 🛡️ ハードニングとHIDセキュリティ

ハンディQRスキャナはOSから「キーボード（HID）」として認識されるため、互換性が高い一方で**HIDキーストローク注入**の経路にもなり得ます。

### 多層的な防御策
1. **ソフトウェア側検証**: Readerは入力データを自動検査し、不正な**ASCII制御文字**（Tab/Escape/GUI系など）を検出した場合は即時中断して攻撃をブロックします。
2. **利用者責任**: スキャナ側で**制御文字・修飾キー送信を無効化**してください。業務用スキャナは設定で「Printable ASCII only」を強制可能です。

> [!IMPORTANT]
> **利用者責任: スキャナ設定**
> OSレベルのリスク軽減のため、スキャナ側で**制御文字・GUI/修飾キー送信を無効化**してください。多くの業務用スキャナは、設定バーコードで「Printable ASCII only」を強制できます。

---

## 🔒 セキュリティ監査担当者向けの技術要点

### 1. Zero-Footprint & Sandboxed Execution
*   インストーラ不要の静的配布。ホストOSのレジストリ痕跡を作りません。ブラウザの保護領域内で完結します。

### 2. Verified Network Isolation
*   **Offline-Ready**: 読み込み後はインターネット非接続でも動作。
*   **No Callbacks**: テレメトリ、外部API呼び出しは一切含みません。

### 3. 透明で監査可能なロジック
*   ロジックは平文のJavaScriptで実装。動的スクリプト読込もなく、全体監査が容易です。

---

## ⚖️ ライセンス / 著作権
Copyright © 2026 Koichiro Watanabe. All rights reserved.
Distributed under the **Apache License 2.0**.
