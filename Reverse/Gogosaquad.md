
This challenge is tricky for me and need times to solve


first the challenge doesn't have a lot of functions , i cant find the important functions like mains and others.

<img width="1920" height="1045" alt="cantfindmain" src="https://github.com/user-attachments/assets/90a8a58f-6144-43bc-8560-2bb0d7d2bc8a" />

so i started to using DIE(Detect it easy) for check it was packed.
then i found the file was unpacked with upx .


<img width="730" height="544" alt="DIE" src="https://github.com/user-attachments/assets/f0f66a92-b860-44c1-8adf-7246efff1911" />

so unpack but can't unpack . So i started sus the hex so i use GHEx for hex edit .
<img width="851" height="255" alt="errorforxxd" src="https://github.com/user-attachments/assets/e8969d65-2c44-4e17-81a3-1cc72d454cb9" />

i found the corrupted hex UDX .that should be UPX
<img width="802" height="602" alt="editinhex" src="https://github.com/user-attachments/assets/30af7d60-662a-4376-9f72-f22b47321d5f" />

i found the P's hex thats 50
<img width="802" height="602" alt="P" src="https://github.com/user-attachments/assets/08bdd2c4-0de0-414a-9dbb-dcb73725bd74" />

change to UPX with editing hex with 50
<img width="802" height="602" alt="edidone" src="https://github.com/user-attachments/assets/c676afa4-984e-43a7-90b7-cab1e93d79c0" />

then i unpack agian and success.
<img width="881" height="284" alt="unpacked" src="https://github.com/user-attachments/assets/d95cc94f-510d-4f14-bd00-3a959b1f4f79" />

find main or important functions in ghidra but now is too many functions so but didn't find the mains . i ask ai and get that was striped function names.

so i use a tool called redress that can use with r2 .

I found the address or mains .
<img width="1226" height="723" alt="finding main" src="https://github.com/user-attachments/assets/5232b4c0-c2df-4090-a413-4084fac1acb9" />


so started analyze the all that functions .

first main.main have flagformat
<img width="1920" height="1045" alt="ucsyctf{" src="https://github.com/user-attachments/assets/3acc993d-d647-4b08-a60a-42224b7c9413" />


and each functions are using different encodings like XOR and others
<img width="1920" height="1045" alt="xorkey" src="https://github.com/user-attachments/assets/edffeda0-bca1-4132-ae76-fbc371a63f66" />


first we analyze the encode0 using ghidra and chatgpt then i decrypt and got the first part
<img width="1922" height="1047" alt="encode" src="https://github.com/user-attachments/assets/607cc4c7-af8c-4b70-aedd-24dab2dc63de" />


then we analyze the other encode1,2,3 and they are all using various methods like cipher and maths operations.I find the each address for hex text and others that need for decode
<img width="1920" height="1045" alt="encode1" src="https://github.com/user-attachments/assets/c1735e60-1a94-44e3-8097-a2ccf06d1161" />

.And decode with ai.
<img width="1922" height="1047" alt="Screenshot_2026-07-09_00-24-55" src="https://github.com/user-attachments/assets/32a9a37d-f4e1-4b8f-a6d2-c3b1bdd35cde" />
<img width="1920" height="1045" alt="encode1" src="https://github.com/user-attachments/assets/491fb591-8875-476d-a211-543fa86e7cc2" />
<img width="1922" height="1047" alt="encode" src="https://github.com/user-attachments/assets/7de88511-bd01-4dd6-94f8-91e3bf1ccbf7" />
<img width="1922" height="1047" alt="finalflag" src="https://github.com/user-attachments/assets/b85c3da0-e3f3-426d-ab9b-d8c71a00269b" />




the final flag was ucsyctf{gogo_squ4d_r3v3rs1ng_ftw}!!
