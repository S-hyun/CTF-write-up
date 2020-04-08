AUCTF 2020 / Cracker_Barrel Write up
======================================
_한글 영어 설명 다 있습니다 ^^_   
_There are explanations in Korean and English:)_

Ghidra에서 연다. main 함수는 다음과 같다.   
Open in Ghidra. this is the function main.

<pre><code>ulong main(void)

{
  int iVar1;
  
  setvbuf(stdout,(char *)0x0,2,0);
  iVar1 = check();
  if (iVar1 == 0) {
    puts("That\'s not it!");
  }
  else {
    print_flag();
  }
  return (ulong)(iVar1 == 0);
}
</code></pre>

check1()로 간다.   
go to check1().

<pre><code>undefined8 check_1(char *param_1)

{
  int iVar1;
  undefined8 uVar2;
  
  iVar1 = strcmp(param_1,"starwars");
  if (iVar1 == 0) {
    iVar1 = strcmp(param_1,"startrek");
    if (iVar1 == 0) {
      uVar2 = 0;
    }
    else {
      uVar2 = 1;
    }
  }
  else {
    uVar2 = 0;
  }
  return uVar2;
}</code></pre>

check_1()의 key는 starwars이다. starwars를 입력하면 성공문자열이 나오고 다음 라운드로 넘어간다.   
the key of check_1() is starwars. typing starwars, a success string appears and move on to the next round.

<pre><code>(base) root@kali:~/Desktop/AUCTF 2020/rev# ./cracker_barrel 
Give me a key!
starwars
You have passed the first test! Now I need another key!</code></pre>

check_2()를 열면 다음과 같다.   
this is check_2()

<pre><code>ulong check_2(char *param_1)

{
  int iVar1;
  size_t sVar2;
  char *__s1;
  int local_20;
  
  sVar2 = strlen(param_1);
  iVar1 = (int)sVar2;
  __s1 = (char *)malloc((long)(iVar1 + 1) << 3);
  local_20 = 0;
  while (local_20 < iVar1) {
    __s1[local_20] = "si siht egassem terces"[(iVar1 + -1) - local_20];
    local_20 = local_20 + 1;
  }
  iVar1 = strcmp(__s1,param_1);
  return (ulong)(iVar1 == 0);
}
</code></pre>

이 ghidra 코드를 이용해서 check_2_key.cpp 파일을 작성해준다.   
Use this ghidra code to create the check_2_key.cpp file.

<pre><code>//check_2_key.cpp
#include <stdio.h>
#include <iostream>
int main()
{
    int user_input_len = 22;
    char* key;
    int i;

    key = (char*)malloc((long)(user_input_len + 1) << 3);
    i = 0;
    while (i < user_input_len) {
        key[i] = "si siht egassem terces"[(user_input_len + -1) - i];
        i = i + 1;
    }
    std::cout << key;
    return 0;
}</code></pre>

코드를 실행시키면 "secret message this is"가 뜬다. check_2의 key는 "secret message this is"이다.   
When you execute the code, "secret message this is" pops up. The key for check_2 is "secret message this is".

<pre><code>You have passed the first test! Now I need another key!
secret message this is
Nice work! You've passes the second test, we aren't done yet!</code></pre>

check_3 함수는 다음과같다.   
this is the function check_3.

<pre><code>ulong check_3(char *param_1)

{
  bool bVar1;
  size_t sVar2;
  void *pvVar3;
  long in_FS_OFFSET;
  int local_5c;
  int local_54;
  int local_48 [4];
  undefined4 local_38;
  undefined4 local_34;
  undefined4 local_30;
  undefined4 local_2c;
  undefined4 local_28;
  undefined4 local_24;
  long local_20;
  
  local_20 = *(long *)(in_FS_OFFSET + 0x28);
  local_48[0] = 0x7a;
  local_48[1] = 0x21;
  local_48[2] = 0x21;
  local_48[3] = 0x62;
  local_38 = 0x36;
  local_34 = 0x7e;
  local_30 = 0x77;
  local_2c = 0x6e;
  local_28 = 0x26;
  local_24 = 0x60;
  sVar2 = strlen(param_1);
  pvVar3 = malloc(sVar2 << 2);
  local_5c = 0;
  while (sVar2 = strlen(param_1), (ulong)(long)local_5c < sVar2) {
    *(uint *)((long)pvVar3 + (long)local_5c * 4) = (int)param_1[local_5c] + 2U ^ 0x14;
    local_5c = local_5c + 1;
  }
  bVar1 = false;
  local_54 = 0;
  while (sVar2 = strlen(param_1), (ulong)(long)local_54 < sVar2) {
    if (*(int *)((long)pvVar3 + (long)local_54 * 4) != local_48[local_54]) {
      bVar1 = true;
    }
    local_54 = local_54 + 1;
  }
  if (local_20 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return (ulong)!bVar1;
}</code></pre>

이 ghidra 코드를 이용해서 check_3_key.cpp 파일을 작성해준다.   
Use this ghidra code to create the check_3_key.cpp file.

<pre><code>//check_3_key.cpp file
#include <stdio.h>
#include <iostream>
int main()
{
    bool bVar1;
    size_t sVar2;
    char pvVar3[11];
    long in_FS_OFFSET;
    int i;

    int param_1[] = { 0x7a, 0x21,  0x21, 0x62, 0x36, 0x7e, 0x77, 0x6e, 0x26, 0x60 };
    sVar2 = 10;
    i = 0;
    while (sVar2 = 10, i < sVar2) {
        pvVar3[i] = ((param_1[i]^0x14)-2);
        printf("%c", pvVar3[i]);
       i = i + 1;
    }
    return 0;
}</code></pre>

코드를 실행시키면 "l33t hax0r"가 뜬다. check_3의 key는 "l33t hax0r"이다.   
When you execute the code, "l33t hax0r" pops up. The key for check_3 is "l33t hax0r".

마지막으로 입력하면 성공문자열과 flag가 뜬다.   
you type, success text and flag appear.
<pre><code>Nice work! You've passes the second test, we aren't done yet!
l33t hax0r
Congrats you finished! Here is your flag!</code></pre>

