AUCTF 2020 / Sora Write up
======================================
_한글 영어 설명 다 있습니다 ^^_   
_There are explanations in Korean and English:)_

encrypt 함수를 살펴본다.   
Look at the encrypt function.

<pre><code>__int64 __fastcall encrypt(__int64 a1)
{
  unsigned __int64 v2; // rax
  int i; // [rsp-20h] [rbp-20h]

  __asm { endbr64 }
  for ( i = 0; ; ++i )
  {
    sub_10D0();
    if ( i >= v2 )
      break;
    if ( (8 * *(char *)(i + a1) + 19) % 61 + 65 != secret[i] )
      return 0LL;
  }
  return 1LL;
}</pre></code>

secret 은 "aQLpavpKQcCVpfcg"이다.   
Secret is "aQLpavpKQcCVpfcg".   

위 소스를 보고 sora.py 파일을 작성한다.   
View the above source and create a sora.py file.   
<pre><code>#sora.py
import string

a = "aQLpavpKQcCVpfcg"
for i in range(len(a)):
    for s in string.printable:
        if (ord(s)*8 + 19) % 61 + 65 == ord(a[i]):
            print(s, end=' ')
            break;</code></pre>
            
파일을 실행하면 75y"72"b5eak"0eG이 출력된다. 75y"72"b5eak"0eG를 입력해주면 flag가 나온다.   
When the file is run, 75y"72"b5eak"0eG is output. Enter 75y"72"b5eak"0eG and you'll get a flag.
<pre>
ubuntu@ubuntu:~/Desktop/AUCTF-2020-master$ nc 127.0.0.1 30004
Give me a key!
75y"72"b5eak"0eG       
auctf{that_w@s_2_ezy_29302}
</pre>

