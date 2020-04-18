AUCTF 2020 / Har_Har_Har Write up
======================================
_한글 영어 설명 다 있습니다 ^^_   
_There are explanations in Korean and English:)_

dev.quickbrownfoxes.org_Archive [19-12-01 15-41-53].har 파일을 hex로 열어 content의 text 부분만 남기고 다 지운다. 그리고 저장한다.   
Open the ha file with hex and erase it all, leaving only the text portion of the content. And save.   

<pre>$ cat dev.quickbrownfoxes.org_Archive\ \[19-12-01\ 15-41-53\]_fix.har | base64 -d > out.png</pre>

명령어를 치면 flag가 써있는 그림이 생긴다.   
When you type the command, you get a picture with a flag on it.
