AUCTF 2020 / Plain_Jane Write up
======================================
_한글 영어 설명 다 있습니다 ^^_   
_There are explanations in Korean and English:)_

<pre>
$gcc -o plain_jane plain_jane.s
$gdb plain_jane
...
(gdb) b *main+49
...
(gdb) r
...
(gdb) info reg
rax            0x6fcf	28623
...
</pre>

rax에 28623이 들어있는 것을 확인할 수 있다.

flag : 28623
