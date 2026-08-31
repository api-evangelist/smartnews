---
title: "Lambda で Cache Invalidate を自動化する"
url: "https://developer.smartnews.com/blog/2015/08/21/20150811lambda-cache-invalidate/"
date: "2015-08-21"
feed_url: "https://developer.smartnews.com/blog/feed"
---
スマートニュース株式会社 の尾形 ( @nobu666 ) です。 インフラ専任エンジニアが一人もいない 弊社ですので、自分もインフラエンジニアと名乗らずに、飲酒系エンジニアとか言っておこうと思っております。 さて、今回は軽めのネタをご紹介させていただこうと思います。弊社では全面的に AWS を採用しており、2015年6月に Lambda が Asia Pacific (Tokyo) のリージョンで利用可能になりましたので早速使ってみました。AWS Lambda の詳細については、 製品ページ をご覧ください。 やりたいこと 弊社ではCDNとして Amazon CloudFront と Akamai Download Delivery を併用しています。その中でも、ニュース記事のサムネイル画像なんかは Amazon S3 を Origin にして画像の配信を行っています。あまりアグレッシブに Cache してしまうと画像の差し替えがあった時に困るのと、そもそも Cache Invalidate を管理画面から手動でやらなくてはならないため面倒です。Lambda が使えるようになったので、だったら S3 にファイルが上がった時点で勝手に Cache Invalidate するようにできれば、アグレッシブな Cache を行ってもいいし、人間が行う作業も減るし一石二鳥というわけです。 
