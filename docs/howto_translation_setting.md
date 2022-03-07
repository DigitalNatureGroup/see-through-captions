# 翻訳APIの作成方法

## 1. Google App Scripts(GAS)にてスクリプトを新規作成する

https://script.google.com にアクセスして，以下のコードをコピペする．

```
function doGet(e) {
    // リクエストパラメータを取得する
    var p = e.parameter;
    //  LanguageAppクラスを用いて翻訳を実行
    var translatedText = LanguageApp.translate(p.text, p.source, p.target);
    // レスポンスボディの作成
    var body;
    if (translatedText) {
        body = {
          code: 200,
          text: translatedText
        };
    } else {
        body = {
          code: 400,
          text: "Bad Request"
        };
    }
    // レスポンスの作成
    var response = ContentService.createTextOutput();
    // Mime TypeをJSONに設定
    response.setMimeType(ContentService.MimeType.JSON);
    // JSONテキストをセットする
    response.setContent(JSON.stringify(body));

    return response;
}
```
[※コード参考元](https://qiita.com/satto_sann/items/be4177360a0bc3691fdf)


## 2. デプロイしてAPIを得る

「デプロイ」⇨「新しいデプロイ」⇨「種類の選択」⇨「ウェブアプリ」⇨「アクセスできるユーザー」を全員にする⇨「デプロイ」

以上の手順を行うとデプロイが完了し，デプロイIDが発行されます．このデプロイIDが本システムにおける翻訳APIとなります．

## 3. 翻訳APIを使う

翻訳機能を使うためには本システムに翻訳APIを読み込ませる必要があります．そのためには以下の二つの方法があります．また，読み込まれたAPIはGoogle Chrome内のlocalStorageに保存され，次回アクセス以降はAPIを読み込ませなくても自動で翻訳機能を使うことができます．

### 3-a. クエリパラメータを使う

アクセス時のURLにクエリパラメータ`translate_api`に対してAPIを渡すことによって読み込ませることが可能です．

### 3-b. 入力フォームを使う

システムの詳細設定内に設置された「翻訳API入力欄」にAPIを渡すことによって読み込ませることが可能です．

---

上記のいずれかの方法にAPIを読み込ませると，翻訳のためのUIが出現し翻訳機能を使うことができるようになります．
