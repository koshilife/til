action mailbox に関するメモ

ガイド
https://railsguides.jp/action_mailbox_basics.html


受信メールの処理をRails側でコントロールできる仕組み。
メール内容を保存する [InboundEmail](https://github.com/rails/rails/blob/master/actionmailbox/app/models/action_mailbox/inbound_email.rb) クラスは
ActionRecordを継承しているため、ActiveRecordをスキップした rails アプリでは インストールに失敗し、ガイド通りでは利用できない。
