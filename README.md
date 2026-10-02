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

## Security

- The Firebase Web API key in `firebaseConfig` is a public client identifier for Firebase services, not an authorization secret.
- Restrict that key in Google Cloud to the Firebase-related APIs required by this app. Do **not** allow the Generative Language API on the Firebase Web API key.
- Protect Firestore with Firebase Security Rules and Firebase App Check. Hiding or obfuscating the Firebase Web API key is not a security control.
- The Gemini API key typed into the UI is not written to Firestore or exported to CSV. Do not commit a Gemini API key to this repository and do not distribute a shared Gemini key to students.
- For a shared classroom deployment of Gemini features, prefer Firebase AI Logic + App Check (or another server-side proxy) so the Gemini credential remains server-side.
- See [SECURITY.md](SECURITY.md) for the deployment checklist.

## License

© 2026 Science Communication Design Laboratory, Kagawa University

Code: MIT License

Documentation and educational materials: CC BY 4.0

The source code is provided under the MIT License. Documentation and educational materials are provided under the Creative Commons Attribution 4.0 International (CC BY 4.0) license.
