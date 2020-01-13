
# Table of Contents

1.  [前情提要](#orgad248bb)
2.  [前情提要2](#orge9072fd)
3.  [[進入正題]關於Erlang裡的Processes](#orgdc03bf5)
4.  [Distributed Programming](#org9f6dd86)



<a id="orgad248bb"></a>

# 前情提要

<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-10 五&gt;</span></span>前一天晚上我被朋友邀請入[Elixir Taiwan](https://www.facebook.com/groups/elixir.tw/)臉書社團，還未等我回答完入團問題，就直接加進去了。
隔天，開始找他聊聊這件事情。Erlang並不是我第一次接觸，不過我並沒有好好去學，主要是我認為，如果要學習
純函數語言的概念，最好的還是 `Haskell` ，一個由基於λ演算，由委員會制定，是非常嚴謹的語言。

[Elixir](https://elixir-lang.org/)是基於Erlang VM的一個語言，有點像是Jython之於Java或是Kotlin之於Java。

※ Erlang我在「[松本行弘的程序世界也有看到](https://www.lagagain.com/post/%E5%BF%83%E5%BE%97%E7%AD%86%E8%A8%98%E6%9D%BE%E6%9C%AC%E8%A1%8C%E5%BC%98%E7%9A%84%E7%A8%8B%E5%BA%8F%E4%B8%96%E7%95%8C%E8%AE%80%E6%9B%B8%E7%AD%86%E8%A8%98/)」，當時還跟我朋友講：(後面查到的)Ping-Pong的例子也正是書裡
有提到的

然後開始聊到這個語言的一些特色，還有六角推坑。其中關於這語言裡一個比較特別的概念&#x2013;Processes，他的認
知與我稍有不同。他也不是長期寫這個語言，但是聽起來他有聽到六角的介紹。我不清楚這推坑到底推了啥，但是
就我先前對於Erlang和作業系統的認知(印象中我確實查過Erlang裡的Processes所代表的意義，當時我可能正在學
Golang)，不太可能是他說的新技術，於是乎我又看了一番(第二次)，這是我的簡短紀錄。


<a id="orge9072fd"></a>

# 前情提要2

我在加入社團後，看了高見龍的「[你看過 Elixir 嗎？如果沒有，現在讓你看看！](https://kaochenlong.com/2017/10/23/have-you-met-elixir/)」，其中Pipe Operator像極了
我寫過得[Pipe Function](https://www.lagagain.com/post/%E7%94%A8python%E5%AF%A6%E7%8F%BEcallable-classfp%E6%9B%B4%E5%A5%BD%E5%AF%AB/#771-jewels-and-stones)。此外，一番調查下，Haskell也有[flow](https://hackage.haskell.org/package/flow-1.0.19/docs/Flow.html)的操作，也很靈活。(近日我可能會重看Hasekll趣
學指南，覺得以前沒看懂的東西現在應該可以懂了)


<a id="orgdc03bf5"></a>

# [進入正題]關於Erlang裡的Processes

我主要參考[Erlang文件裡的Concurrent Programming](https://erlang.org/doc/getting_started/conc_prog.html)。在我的認知裡面，Erlang的Processes，也就是Java語言級
別提供的Thread，或是Golang裡的gorountine，Lua的corountine。或者又被稱作User-Thread、lightweight
thread，或是Green Thread（朋友提出來的，這個我看到有印象，但一開始搞錯意思😅）。這個層級的Thread對應
到系統層級的，可能是一對一、一對多、多對多。

我也就以該文件內容來支持我的觀點。(也不是要說服人拉，單純我自己也忘記好奇，紀錄一下)

其中寫到：

> It is easy to create parallel threads of execution in an Erlang program and to allow these threads
> to communicate with each other.  **In Erlang, each thread of execution is called a process.**
> 
> (Aside: the term "process" is usually used when the threads of execution share no data with each other and the term "thread" when they share data in some way. Threads of execution in Erlang share no data, that is why they are called processes).

意思是，Erlang裡的Thread被稱作Process（雖然一般作業系統裡談的Process，是獨立一隻程式在記憶體的狀態）。
主要原因是，Thread有共享資料，Process沒有，而Erlang的Thread也不共享資料，所以直接稱呼Process。不過這
也是為什麼我說到，一般 **純函式型語言** 都有很強、很容易平行處理的能力，因為他們純、不改變外部狀態、通常
給予相同輸入會得到相同輸出，因為不共享資料，所以不會碰到資料搶佔衝突問題。

附帶一題，一般不同程序間的溝通方式有(這問題還曾經被我博班學長請教到)：

-   共享記憶體
    以Linux來說，可以用mmap將記憶體映射到一份虛擬檔案上，使用類似檔案存取的方式。
-   signal（訊號）
    透過系統訊號，貌似也是Erlang裡模擬的方式。
-   標準輸入輸出
    Linux的pipe就是一例。
-   網路
-   （當然還有檔案系統，檔案交換）


<a id="org9f6dd86"></a>

# Distributed Programming

其實該文件有寫到我認為這最有作用的應用 &#x2013; [Distributed Programming](https://erlang.org/doc/getting_started/conc_prog.html#distributed-programming)。不過不好意思本人的英文能力沒辦法
長時間讀，偶會受不了wwww。以後有想到在回去看看吧哈哈。

不過這事情，在後端曾經有小小討論過。我自己也有一些切身經驗&#x2026;總結而言，想用數百、數千、數萬個Thread
提高程式運行&#x2026;作夢吧，還是存在作業系統與硬體本身的限制。除非使用多台電腦突破限制，分散處理。但是使
用Thread的概念，無疑在一些環境下的開發，是適合的邏輯。

