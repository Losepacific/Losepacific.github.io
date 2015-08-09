---
layout: post
title: "python3伺服器提供cgi的功能"
date: 2015-07-22 01:43:10 +0800
comments: true
categories: Python
---
Python3[^why_python]可以藉由下方指令，把當前的目錄做為網頁伺服器的根目錄，運行網頁服務：

[^why_python]:本來練習網頁一直都是用python當作簡易伺服器，但是直到需要使用php，才發現python不會自動去運行php的內容，雖然這個問題只要改用php自身的簡易伺服器就可以解決了：<code>php -S 127.0.0.1:8000 -t.</code>。但是，總會不服氣，為何php可以，python沒道理不行。

``` python
python3 -m http.server  8000 #開啟網路服務於 Port 8000
```

但是上述指令不包括支援CGI的功能，如果需要CGI的功能，必需使用：

``` python
python3 -m http.server --cgi 8000 
```
<!-- more -->

不過，CGI的檔案不是任何地方都可以放置，基本上Python3的模組，預設需放在當前目錄下的<code>cgi-bin</code>或<code>htbin</code>這兩個目錄中[^python3_cgi]， 才會被視為CGI，否則，會當作檔案直接下載，或者，以文字的方式直接顯示檔案裡的程式內容[^detail]。

[^detail]:以<code>.pl</code>或<code>.py</code>結尾的副檔名 會顯示內容；<code>.php</code>則會直接下載檔案。

[^python3_cgi]:python3的<code>http.server</code>模組預設<code>http.server.CGIHTTPRequestHandler.cgi_directories=['/cgi-bin', '/htbin']</code>，如果想要改動放cgi程式的資料夾，就得重新設定這個屬性，比如說改成<code>CGI</code>這個目錄：<code>http.server.CGIHTTPRequestHandler.cgi_directories=['/CGI']</code>

檔案目錄的結構示意圖：

<object type="image/svg+xml" data="/images/cgi_dir_struct.svg">Your browser does not support SVG</object>

另外，CGI裡，輸出的前兩行文字必需含有如下內容：

``` perl
print "Content-Type: text/plain \n\n";  #以Perl為例
```

或者：

``` perl
print "Content-Type: text/html \n\n";
```

也就是說，輸出的內容必需有<code>Content-Type: text/html</code>外加一個空行[^blank_line]。 不然，瀏覽器只會獲得一個空白的內容。

[^blank_line]:當初看python3的範例上寫<code>print("Content-Type: text/html\n")</code>就自作聰明的以為python3預設是會在字串結束的地方加上換行符號，應該不用再多餘的添加<code>\n</code>，結果沒想到是還多需要一個空白行，而卡在這邊很久。

然後，必需把檔案改為可執行檔。

這邊就以一個php檔案作為CGI內容的例子為結尾：

``` php
#!/usr/bin/env php
<?php
echo "Content-Type: text/html\n\n";
 
echo "<html>";
 
echo "<head>";
echo "<title>CGI TEST</title>";
echo "</head>";
 
echo "<body>";
echo "<h1>CGI TEST</h1>";
echo "<p>這是一個CGI的網頁測試。火腿。荷包蛋。蕃茄醬。</p>";
echo "</body>";
 
echo "</html>";
?>
```  
