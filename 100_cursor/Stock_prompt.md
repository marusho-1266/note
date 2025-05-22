// 変数定義
const workflow_PATH = "100_Cursor/workflow.md"
const INBOX_PATH = "00_Inbox/Clip/";
const LIT_PATH = "20_Stock/";
// 指示
@file(${workflow_PATH})を参照し、
@dir(${INBOX_PATH})の全ての記事をLiterature形式して
メタデータを付与し @dir(${LIT_PATH}) に保存