# hatenablog-auto-play
はてなブログ記事内の「iTunes商品紹介」を自動で再生するためのコードスニペット。

以下のコード（index.min.html）をブログ記事に貼り付けると、画面右下に自動再生ボタンが表示され、このボタンを押すことで自動再生が有効になる。自動再生が有効な状態では、画面で見えている「iTunes商品紹介」が再生される。また、スクロールにより画面内に新しい「iTunes商品紹介」が表示されると、自動で再生対象が切り替わる。もう一度ボタンを押すと自動再生が無効となる。

```html
<span></span><style>
    #autoplay-btn{color:gainsboro;background-color:#fff;background-size:cover;position:fixed;bottom:5%;right:10%;width:60px;height:60px;border:5px solid currentColor;border-radius:100%;cursor:pointer;z-index:10000}#autoplay-btn.disabled{opacity:.3;cursor:not-allowed}#autoplay-btn:before,#autoplay-btn:after{content:"";position:absolute;top:10px;left:30%;border-top:20px solid transparent;border-left:35px solid currentColor;border-bottom:20px solid transparent}#autoplay-btn.autoplay{border-style:inset}#autoplay-btn.autoplay:before,#autoplay-btn.autoplay:after{opacity:.5;height:40px;border-width:0 6px 0 6px;border-color:transparent currentColor transparent currentColor}#autoplay-btn.autoplay:after{left:60%}
</style>
<div id="autoplay-btn" class="disabled"></div>
<script>
let observer=null;document.addEventListener("DOMContentLoaded",()=>{const autoplayBtn=document.getElementById("autoplay-btn");autoplayBtn.classList.remove("disabled"),autoplayBtn.addEventListener("click",e=>{autoplayBtn.classList.contains("autoplay")?autoplayOff(e.target):autoplayOn(e.target)})});const autoplayOn=btn=>{btn.classList.add("autoplay");const audios=btn.parentNode.getElementsByTagName("audio");for(const audio of audios)audio.load(),audio.loop=!0;observer=new IntersectionObserver(entries=>{let activeAudio=null;if(entries.forEach(entry=>{entry.target.paused&&!entry.isIntersecting||(activeAudio=entry.target)}),null!==activeAudio){for(const audio of audios)!audio.isSameNode(activeAudio)&&audio.pause();activeAudio.play();const img=activeAudio.closest(".itunes-embed").querySelector("img");btn.style.backgroundImage=img?`url(${img.getAttribute("src")})`:"none"}},{rootMargin:"-30% 0px"});for(const audio of audios)audio.load(),observer.observe(audio)},autoplayOff=btn=>{btn.classList.remove("autoplay"),btn.style.backgroundImage="none",observer&&observer.disconnect();const audios=btn.parentNode.getElementsByTagName("audio");for(const audio of audios)audio.pause()};
</script>
```

デモとコードの解説は以下の記事へ。

[IntersectionObserverではてなブログ記事内のApple Musicを自動再生する](https://mtzml.hatenablog.com/entry/2023/02/04/155550)