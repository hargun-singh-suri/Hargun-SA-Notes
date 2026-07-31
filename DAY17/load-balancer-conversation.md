**Junior:** Sir, ab tak humne requirements, quality attributes, aur API design cover kar liya. Ab agla topic kya hai?

**Senior:** Ab hum enter kar rahe hain software architecture ke building blocks wale section mein. Pehla aur sabse important building block hai — ek system jo traffic ko alag alag servers mein baant deta hai (**Load Balancer**). Almost har real-life large-scale system isko use karta hai.

**Junior:** Load balancer ki zaroorat kyun padti hai?

**Senior:** Yaad hai humne pehle seekha tha — system hamesha available rahe (**availability**) aur zyada machines add karke handle ho sake (**horizontal scalability**), iske liye hum apni application ki multiple identical copies (**instances**) multiple computers pe chalate hain. Par problem ye hai — agar traffic baantne wala system na ho, toh client application ko khud pata hona chahiye ki hamare kitne servers hain aur unke addresses kya hain.

**Junior:** Isse dikkat kya hai?

**Senior:** Isse client application humare system ke andar ki banawat (**internal implementation**) se bahut zyada juda ho jaata hai (**tightly coupled**) — matlab agar hum servers add/remove karein, client ko bhi update karna padega. Isliye traffic baantne wala ye system ye problem solve karta hai — ye traffic ko servers ke group mein baantata hai, taaki koi ek server overload na ho, aur saath mein poore server group ko client ke liye ek single, powerful server jaisa dikhata hai.

**Junior:** Load balancer se kya kya achi khoobiyaan (**quality attributes**) milti hain?

**Senior:** Chaar important cheezein milti hain:
1. Zyada machines add karke badhne ki taakat (scalability)
2. Hamesha available rehna (availability)
3. Speed/kaam karne ki raftaar (performance)
4. Aasani se maintain kar paana (maintainability)

Chalo inko ek ek karke dekhte hain.

**Junior:** Scalability wala kaise kaam karta hai?

**Senior:** Servers ko load balancer ke peeche hide karke, hum zyada machines add ya kam kar sakte hain (**horizontally scale**) — load badhne pe servers add karo, load kam hone pe servers remove kar do (paisa bachta hai). Cloud mein toh khud-ba-khud servers add/remove karne wali policies (**auto-scaling policies**) bhi use kar sakte hain jo criteria ke basis pe (jaise requests per second, bandwidth) automatically kaam karti hain.

**Junior:** Hamesha available rehna kaise improve hota hai?

**Senior:** Load balancer ko configure kar sakte ho ki wo un servers ko traffic bhejna band kar de jo pahunch mein nahi hain (**unreachable**). Iske dekh-rekh wale feature (**monitoring**) se, wo sirf theek-thaak (**healthy**) servers ko hi traffic bhejta hai, marey hue ya slow servers ko ignore kar deta hai.

**Junior:** Performance pe kya asar padta hai?

**Senior:** Thoda sa deri (**latency**) add hota hai (ye acceptable trade-off hai), par kaam karne ki total raftaar (**throughput**) kaafi badh jaati hai — kyunki tumhare paas ek server ki jagah bahut saare backend servers hote hain, toh total requests jo handle ho sakti hain per unit time, kaafi badh jaati hain.

**Junior:** Aur aasani se maintain karna?

**Senior:** Servers ko baari-baari (**rotation**) se ek ek karke nikal sakte ho maintenance ya upgrade ke liye, bina client ko disturb kiye. Maintenance khatam hone ke baad wapas add kar do, aur next server ke saath repeat karo. Isse ek ke baad ek karke naya version release karna (**rolling release**) possible hota hai, aur availability ka wada (**SLA**) bhi maintain rehta hai.

**Junior:** Ab load balancer ke kitne types hote hain?

**Senior:** Chaar types hote hain:
1. Domain naam se traffic baantna (DNS load balancing)
2. Ek dedicated machine (Hardware load balancer)
3. Ek software program (Software load balancer)
4. Poori duniya mein traffic baantne wala (GSLB — Global Server Load Balancer)

Chalo ek ek karke dekhte hain.

**Junior:** DNS load balancing kaise kaam karta hai?

**Senior:** DNS matlab wo system jo naam ko address mein badalta hai (**Domain Name System**) — ye internet ka "phonebook" hai, jo human-friendly domains (jaise amazon.com) ko IP addresses mein map karta hai. Ek DNS record multiple IP addresses mein map ho sakta hai. DNS servers usually ye list har request pe different order mein return karte hain (ek baari-baari wala tareeka — **round-robin**), aur client simply pehla address pick kar leta hai. Isse naturally load spread ho jaata hai.

**Junior:** Iske koi drawbacks hain?

**Senior:** Haan, kaafi hain:
1. DNS server ye check nahi karta server theek hai ya nahi (**health monitoring**) — agar ek server down ho jaaye, DNS ko pata hi nahi chalega, aur wo clients ko wahi bhejta rahega.
2. Address list yaad rakhi jaati hai (**cache**) ek fixed samay (**TTL — Time To Live**) ke basis pe update hoti hai — matlab ek marey hue server ko traffic milna kaafi der tak continue ho sakta hai.
3. Load baantne ka tareeka sirf simple baari-baari wala hai — ye account nahi karta ki koi server zyada powerful hai ya koi zyada overloaded hai.
4. Suraksha ka issue (**security issue**) — client ko saare servers ke direct IP addresses mil jaate hain, jo andar ki banawat expose karta hai. Koi badnium (**malicious**) client ek hi IP pe requests bhejke usko intentionally overload kar sakta hai.

**Junior:** Toh inhe fix karne ke liye kya hai?

**Senier:** Isi liye ek dedicated machine (Hardware load balancer) aur ek software program (Software load balancer) use karte hain — dono kaafi behtar aur samajhdar (**intelligent**) hote hain.

**Junior:** Inme farak kya hai?

**Senior:** Sirf ye — Hardware load balancer ek dedicated physical machine pe chalta hai, jo specifically load baantne ke liye design ki gayi hai. Software load balancer sirf ek program hota hai jo kisi bhi general computer pe chal sakta hai, same kaam karte hue.

**Junior:** Ye DNS ke drawbacks kaise fix karte hain?

**Senior:** Kai tareekon se:
1. Client aur servers ke beech saara communication load balancer ke through hota hai — individual server IPs kabhi expose hi nahi hote, isliye suraksha kaafi behtar hoti hai.
2. Ye baar baar server ko check karte hain (**health checks**) taaki failures actually pata chal sakein, sirf andaza na laga rahe hon.
3. Ye load ko samajhdari se baantate hain (**intelligently**) — server ki hardware, current load, kitne connections khule hain — sab consider karte hue.

**Junior:** Inka koi aur use bhi hai apart from bahar se aane wale traffic ke?

**Senior:** Haan, ye system ke andar bhi (**internal services**) use ho sakte hain. Jaise ek online store mein, user ko dikhne wali service (**front-end**) ko order poori karne wali service (**fulfillment service**) aur billing service se alag kiya jaa sakta hai, aur wo saare aapas mein load balancer ke through baat karein. Isse har service alag alag aur bina doosre pe asar dale (**independently, transparently**) badh sakti hai (**scale**).

**Junior:** Inka koi limitation bhi hai?

**Senior:** Haan, ye usually apne servers ke bilkul paas rakhne padte hain (**collocated**) — kyunki agar load balancer bahut door ho, toh extra deri (latency) add ho jaayegi. Isliye agar tum ek se zyada bhoogolik jagahon pe (**multiple geographical data centers**) system chala rahe ho, ek hi load balancer dono jagah ke liye kaafi nahi hoga.

**Junior:** Toh alag alag jagahon pe chalne wale systems ke liye kya solution hai?

**Senior:** Yahi pe aata hai chautha type — poori duniya mein traffic baantne wala system (**GSLB — Global Server Load Balancer**). Ye ek mix hai DNS aur hardware/software load balancer ka.

**Junior:** GSLB kaam kaise karta hai?

**Senior:** GSLB normal DNS service jaisa hi kaam karta hai, par saath mein samajhdari se route karne ka faisla bhi leta hai (**intelligent routing**):
1. Ye user kaha se aa raha hai wo pata kar sakta hai unke request ke asli address (**origin IP**) se.
2. Iske paas dekh-rekh karne ki capability (**monitoring**) bhi hoti hai jaise ek normal load balancer ke, matlab isko pata hota hai kaunsa data center theek hai aur kaha hai.

Jab user DNS query bhejta hai, GSLB unhe sabse nazdeek theek-thaak (**nearby healthy**) load balancer/data center ka address de deta hai.

**Junior:** Kya GSLB sirf location ke basis pe hi route karta hai?

**Senior:** Nahi, GSLB kai tareekon se route kar sakta hai — jaise current traffic ya processing load (**CPU load**) har data center pe, ya best anumaanit jawab dene ka samay (**estimated response time**) ya data transfer ki gati (**bandwidth**) user aur data center ke beech. Isse har user ko best possible performance milta hai, unki location kuch bhi ho.

**Junior:** Ye aapatkaalin sthiti mein bhi kaam aata hai (**disaster recovery**)?

**Senior:** Bilkul, agar ek data center down ho jaaye (natural disaster ya bijli jaane ki wajah se — **power outage**), users automatically doosri location pe route ho jaate hain — jo hamesha available rehne ki khoobi (**high availability**) provide karta hai.

**Junior:** Aur agar load balancer khud hi akela nakami ka karan (**single point of failure**) ban jaaye?

**Senior:** Iske liye har region mein multiple load balancers rakh sakte ho, aur unke saare addresses GSLB ke DNS service mein register kar sakte ho. Client applications ko saari list milegi, aur wo pehla address pick kar sakte hain ya randomly koi bhi choose kar sakte hain.

**Junior:** Samjha sir — matlab load balancer sirf traffic baantne wala nahi hai, ye badhne ki taakat, hamesha available rehna, performance, aur aasani se maintain karna — sab ko enable karta hai — aur GSLB toh isko poori duniya tak le jaata hai.

**Senior:** Bilkul sahi samjhe.

**ONE LINE STAFF ANSWER:** Load balancer traffic ko servers ke group mein baantata hai aur unke details client se chhupata hai — DNS wala tareeka simple hai par "andha" hai (health check nahi karta), Hardware/Software load balancers active monitoring aur samajhdari se routing dete hain ek data center ke andar, aur GSLB DNS aur load balancer dono ki taakat milake users ko poori duniya mein sabse nazdeek theek-thaak data center tak route karta hai.
