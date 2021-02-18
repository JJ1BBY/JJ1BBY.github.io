# Welcome to JJ1BBY blog

## ごあいさつ

趣味のTwitter無線垢@JJ1BBYでは、なるべく楽しい話題、批判なし問題提起Ok、政治・宗教・陰謀説等の押し付け無用を信条に無線・登山(山岳移動)・電子工作・ガジェオタ関連でつぶやいています。   
トラブル防止のため鍵垢にしております。鍵垢お気に召さない方はスルー、ミュート、ブロックご自由にお願いします。 

フォローリクエストいただいた場合は、TLを拝見させていただき、上述の趣旨に沿っているか確認させていただいています。  

FF外からのDM許可しております。何かあればDMもしくはJARLメール経由でご連絡ください。  

### 当blogで扱う内容  
基本的にアマチュア無線のトピックで、記録しておきたい内容。少しでも、ご参考になりそうな内容を投稿しています。  

### QSL発行ポリシー
  
- コンテストの際のQSLはコンテスト中に「QSL交換」とご指定下さい。時間がかかりますが、発行いたします。ご指定ない場合は発行していません。
- その他の運用時は基本発行しております。ノーカードの交信も大歓迎ですので、ぜひよろしくお願いします。  
  
聞こえておりましたら交信よろしくお願いします。  


----
## [投稿一覧](/allposts.md/)

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}


----  
  
<script src="https://utteranc.es/client.js"
        repo="JJ1BBY/JJ1BBY.github.io"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
<a href="{{ "/Privacy-Policy.html" }}"プライバシー ポリシー</a>
