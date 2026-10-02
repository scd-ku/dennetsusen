# 電熱線データ集計ボード

電熱線を使った理科実験の測定データを入力・共有し、黒板風のボードで集計できるWebアプリです。Gemini APIを利用したカテゴリ分類にも対応しています。

## Webアプリ

https://scd-ku.github.io/dennetsusen/

## 主な機能

- 班、ニクロム線の太さ・長さ、電池のつなぎ方の記録
- 開始時／終了時の水温・電圧・電流の記録
- 上昇温度と電力の表示
- Firebaseを利用した共有ボード
- いいね、個別削除、全データ削除
- CSV形式でのデータ保存
- Gemini APIによるカテゴリ分類
- 参加用QRコードとアプリ内ヘルプ

## 授業ルーム方式

このアプリはGoogleログインや教員の事前承認を必要としません。

1. 教員が「新しい授業を始める」を押すと、6文字の授業コードを持つ専用ルームを作成します。
2. Firebase Anonymous Authentication が、そのブラウザを授業作成者（教員）として識別します。
3. 生徒は授業コードまたはQRコードから参加します。ログイン操作は不要です。
4. 生徒はデータ入力・閲覧・いいねができます。
5. 授業を作成した教員だけが、Gemini分類・CSV保存・個別削除・全削除・いいねリセットを実行できます。
6. データは `rooms/{roomId}/notes` に保存され、授業ごとに分離されます。

> **重要:** 教員権限は授業を作成したブラウザの匿名Firebase IDに紐づきます。ブラウザのサイトデータを削除したり別端末へ移ったりすると、その授業の教員権限は復旧できません。必要なデータは授業終了時にCSV保存してください。

## Firebase 初回設定

このバージョンでは、Firebase Console側で次の2点を一度だけ設定する必要があります。

- Authentication → Sign-in method で **Anonymous** を有効化
- このリポジトリの `firestore.rules` を対象Firebaseプロジェクトへデプロイ

Firebase CLIを利用する場合は、対象プロジェクトを選択したうえで次を実行できます。

```bash
firebase deploy --only firestore:rules
```

## Security

- The Firebase Web API key in `firebaseConfig` is a public client identifier for Firebase services, not an authorization secret.
- Restrict that key in Google Cloud to the Firebase-related APIs required by this app. Do **not** allow the Generative Language API on the Firebase Web API key.
- Protect Firestore with the room-based rules in `firestore.rules` and, where practical, Firebase App Check. Hiding or obfuscating the Firebase Web API key is not a security control.
- The Gemini API key typed into the UI is not written to Firestore or exported to CSV. Do not commit a Gemini API key to this repository and do not distribute a shared Gemini key to students.
- For a shared classroom deployment of Gemini features, prefer Firebase AI Logic + App Check (or another server-side proxy) so the Gemini credential remains server-side.
- See [SECURITY.md](SECURITY.md) for the deployment checklist.

## License

© 2026 Science Communication Design Laboratory, Kagawa University

Code: MIT License

Documentation and educational materials: CC BY 4.0

The source code is provided under the MIT License. Documentation and educational materials are provided under the Creative Commons Attribution 4.0 International (CC BY 4.0) license.
