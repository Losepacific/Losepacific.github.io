---
layout: post
title: "python3伺服器提供cgi的功能"
date: 2015-07-22 01:43:10 +0800
comments: true
categories: Python
---
Python3可以藉由下方指令，把當前的目錄做為網頁伺服器的根目錄，運行網頁服務：

``` python
python3 -m http.server  8000 #開啟網路服務於 Port 8000
```

但是上述指令不包括支援CGI的功能，如果需要CGI的功能，必需使用：

``` python
python3 -m http.server --cgi 8000 
```
<!-- more -->

不過，CGI的檔案不是任何地方都可以放置，基本上需被放置於當前目錄下的<code>cgi-bin</code>或<code>htbin</code>這兩個目錄中， 才會被視為CGI，否則，會被當作檔案直接下載，或者，以文字的方式直接顯示檔案裡的程式內容。(以<code>.pl</code>或<code>.py</code>結尾的副檔名 會顯示內容；<code>.php</code>則會直接下載檔案)

另外，CGI裡，輸出的前兩行文字必需含有如下內容之一：

``` perl
print "Content-Type: text/plain \n\n";  #以Perl為例
```

或者：

``` perl
print "Content-Type: text/html \n\n";
```

也就是說，輸出的內容比需有Content-Type: text/html外加一個空行。 不然，瀏覽器只會獲得一個空白的內容。

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
