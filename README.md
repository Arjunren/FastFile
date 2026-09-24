# FastFile

FastFile is a lightweight, temporary file-sharing web app. Upload one or more files, receive a private six-character key, and use that key to open or download the files for the next 24 hours.

## What it supports

- Any browser-selectable file type: documents, text files, PDFs, images, archives, and more.
- A maximum of **1 MB (1,000,000 bytes) per file**.
- Multiple files in a single share.
- A private, randomly generated six-character key.
- Automatic client-side expiry checks after 24 hours.

FastFile preserves the original file bytes, so a downloaded file keeps its original format. It no longer treats every upload as plain text.

## Run locally

This project is a single static page. Open `index.html` in a modern browser, or serve the folder with a simple local web server.

The Firebase configuration is included in `index.html`; it connects to the existing `fastfile-2ceff` Firestore project. For a separate deployment, replace that configuration with your own Firebase web app settings and deploy the Firestore rules in `firestore.rules`.

## How file storage works

Firestore has a 1 MiB limit for each document. FastFile converts an upload to Base64 in the browser and stores it in small chunks below that limit. The file metadata is stored separately, then the original bytes are rebuilt when the recipient opens or downloads it.

This is why the app can accept any file type while enforcing the 1 MB file limit.

## Security and operations

- A six-character key is a convenience feature, not end-to-end encryption. Do not use FastFile for passwords, private keys, or sensitive personal information.
- Enable Firebase App Check and deploy restrictive Firestore rules before a public launch.
- Expired data is hidden by the app, but Firestore does not automatically delete subcollections when a parent document expires. Use a scheduled Firebase/Cloud Function cleanup job to remove expired shares and their file chunks.
- Keep the 1 MB per-file limit. Larger values can exceed Firestore’s document-size limit after Base64 encoding.

## Project files

- `index.html` — user interface and browser-side upload/download logic.
- `firestore.rules` — recommended Firestore access controls for the app.
- `LICENSE` — MIT License.

## License

MIT. See [LICENSE](LICENSE).
