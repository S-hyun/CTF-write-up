AUCTF 2020 / mobile1 Write up
======================================
_한글 영어 설명 다 있습니다 ^^_   
_There are explanations in Korean and English:)_

<pre><code>ubuntu@ubuntu:~/Desktop/AUCTF-2020-master/Reversing/mobile1$ foremost mobile1.ipa 
Processing: mobile1.ipa
|foundat=Payload/PK
*|
ubuntu@ubuntu:~/Desktop/AUCTF-2020-master/Reversing/mobile1$ cd output/
ubuntu@ubuntu:~/Desktop/AUCTF-2020-master/Reversing/mobile1/output$ cd zip/
ubuntu@ubuntu:~/Desktop/AUCTF-2020-master/Reversing/mobile1/output/zip$ unzip 00000000.zip 
</code></pre>

위와 같이 해주면 Payload 폴더가 생기는데, Payload/mobile1.app/Info.plist를 HxD로 열면 플래그를 찾을 수 있다.   
If you do as above, you will get a Payload folder. You can find the flag by opening Payload/mobile1.app/Info.plist in HxD.


auctf{i0s_r3v3rs1ng_1s_1nt3r3st1ng} 
