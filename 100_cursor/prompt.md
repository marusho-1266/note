// 変数定義
const workflow_PATH = "100_Cursor/workflow.md"
const INBOX_PATH = "00_Inbox_Clip/";
const LIT_PATH = "20_Stock/";
// 指示
@file(${workflow_PATH})を参照し、
@dir(${INBOX_PATH}) の~に関連する記事をLiterature形式して
メタデータを付与し @dir(${LIT_PATH}) に保存


// 変数定義
const LIT_PATH = "20_Stock/";
const DRAFT_PATH = "30_draft/"

// 指示
@dir(${LIT_PATH}) の~に関連する記事についてすべてまとめ、はてなブログ用の記事素案を @dir(${DRAFT_PATH}) に保存してください
