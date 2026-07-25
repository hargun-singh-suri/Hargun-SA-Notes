**Junior:** Sir, pichli lecture mein humne availability samjha. Aaj iska next part hai kya?

**Senior:** Bilkul, aaj hum dekhenge ki failures ko rokte kaise hain aur high availability achieve kaise karte hain. Pehle samajhte hain failures aati kaha se hain. Generally teen sources hote hain:
1. Human error
2. Software errors
3. Hardware failures

Chalo inko detail mein dekhte hain.

**Junior:** Human error se kya matlab hai?

**Senior:** Jaise koi galat config production mein push kar diya, galat command ya script run kar di, ya ek naya software version deploy kar diya jo properly test hi nahi hua tha.

**Junior:** Aur software errors?

**Senior:** Isme aate hain bahut lambi garbage collection pauses, aur crashes jaise out-of-memory exceptions, null pointer exceptions, segmentation faults — waghera.

**Junior:** Aur hardware failures?

**Senior:** Servers, routers, ya storage devices apni age ki wajah se kharab ho jaana, natural disasters ki wajah se power outage, ya infrastructure issues/congestion ki wajah se network fail ho jaana.

**Junior:** Toh in sab failures ko hum kaise handle karein?

**Senior:** Dekho, chahe tum code review, testing, release process, aur hardware maintenance kitna bhi improve kar lo, **failures hoti hi rahengi** — inhe pura rokna possible nahi hai. Isliye asli solution hai **fault tolerance**.

**Junior:** Fault tolerance kya hota hai?

**Senior:** Fault tolerance matlab system ki ability ki wo operational aur available rahe users ke liye, chahe uske ek ya zyada components fail ho jaayein. Aisi situation mein system full performance pe chal sakta hai ya reduced performance pe, par wo **completely unavailable** nahi hota.

**Junior:** Fault tolerance achieve karne ke liye kya tactics use karte hain?

**Senior:** Teen major tactics hote hain:
1. Failure prevention
2. Failure detection aur isolation
3. Recovery

Chalo inko ek ek karke dekhte hain, detail mein.

**Junior:** Pehle failure prevention samjhaiye.

**Senior:** Iska core idea hai — system mein koi bhi **single point of failure** na ho. Jaise agar tumhara poora app ek hi server pe chal raha hai, ya poora data ek hi database instance pe store hai — ye dono single points of failure hain.

**Junior:** Toh inko kaise eliminate karte hain?

**Senior:** **Replication aur redundancy** se. Matlab:
1. App ki multiple copies alag alag servers pe chalao — agar ek server down ho jaaye, traffic doosre servers pe bhej do.
2. Database ki bhi multiple replicas rakho same data ke saath — ek replica lose hone se data lose nahi hoga.

**Junior:** Ye toh spatial redundancy hui na, alag machines pe copies rakhna?

**Senior:** Bilkul sahi. Ek aur type hai — **time redundancy**, jisme same operation ya request ko multiple baar repeat karte hain jab tak wo succeed na ho ya hum give up na kar dein. Ye especially computations ke liye useful hai.

**Junior:** Replication ke liye kya strategies hoti hain?

**Senior:** Do main strategies hain:
1. **Active-Active architecture** — requests saari replicas ko jaati hain, jo unhe hamesha sync mein rehne ke liye force karta hai. Agar ek replica down ho jaaye, baaki turant requests handle karna shuru kar dete hain.
2. **Active-Passive architecture** — ek primary replica saari requests leta hai, aur baaki passive replicas periodic snapshots leke primary ko follow karte hain.

**Junior:** In dono ke pros aur cons kya hain?

**Senior:** Active-Active ke pros: load saari replicas mein baant jaata hai (jaise horizontal scalability), isliye zyada traffic handle ho sakta hai aur performance behtar hoti hai. Cons: saari active replicas ko sync mein rakhna easy nahi hai — coordination aur overhead badh jaata hai.

Active-Passive ke pros: implement karna kaafi easy hai kyunki ek hi clear leader hota hai jiske paas most up-to-date data hota hai. Cons: scaling nahi ho paati, kyunki saari requests ek hi machine ko jaati hain.

**Junior:** Samjha sir. Ab dusra tactic — failure detection aur isolation. Ye kaise kaam karta hai?

**Senior:** Agar app multiple instances mein alag alag computers pe chal rahi hai, aur ek instance crash ho jaaye (software ya hardware issue ki wajah se), toh hume us instance ko detect karke baaki group se isolate karna hota hai — matlab uspe requests bhejna band kar dena.

**Junior:** Isko detect kaise karte hain?

**Senior:** Usually iske liye ek separate **monitoring service** hoti hai, jo do tareekon se kaam karti hai:
1. **Health checks** — monitoring service periodically har instance ko ping karti hai.
2. **Heartbeats** — healthy instances khud periodically "main theek hoon" wala message bhejte hain monitor ko.

Agar monitoring service ko ek server se kisi fixed duration tak koi response nahi milta, toh wo maan leti hai ki wo server down hai.

**Junior:** Agar galti se healthy server ko faulty samajh liya jaaye, tab?

**Senior:** Wo "false positive" hai, aur wo chalta hai — problem nahi hai. Par "false negative" bahut bura hota hai — matlab server actually crash ho chuka hai, par monitoring system ko pata hi nahi chala. Isliye monitoring system false negatives avoid karne pe focus karta hai, false positives thoda tolerate ho sakte hain.

**Junior:** Kya monitoring aur bhi complex ho sakti hai simple pings/heartbeats se?

**Senior:** Haan bilkul, monitoring system aur cheezein bhi track kar sakta hai:
1. Har host per minute kitne errors/exceptions de raha hai — agar suddenly bahut zyada ho jaayein, samjho failure hai.
2. Har host request ko respond karne mein kitna time le raha hai — agar wo time bahut zyada ho jaaye, samjho kuch galat hai.

**Junior:** Samjha sir. Ab teesra tactic — recovery. Iska kya role hai?

**Senior:** Yaad hai pichli lecture ka MTBF/MTTR formula? Wahi principle yaha bhi apply hota hai — agar hum kisi failure ko user ke notice karne se pehle hi detect aur recover kar lein, toh system practically high availability provide karta hai, chahe failures kitni bhi baar hoti hon.

**Junior:** Toh recovery ke liye kya actions le sakte hain?

**Senior:** Faulty instance detect aur isolate hone ke baad, kuch actions le sakte hain:
1. Us instance pe traffic bhejna band kar do — sabse pehla step.
2. Usko restart karne ki koshish karo — ho sakta hai restart ke baad problem chali jaaye.
3. **Rollback** karo — matlab pichli stable aur correct version pe wapas chale jao.

**Junior:** Rollback ka koi practical example dijiye.

**Senior:** Databases mein rollback bahut common hai — agar data integrity violate ho jaaye kisi wajah se, hum pichli correct state pe rollback kar dete hain. Similarly, agar naya software version deploy karne ke baad servers pe bahut zyada errors aane lagein, hum automatically purane stable version pe rollback kar sakte hain, taaki system completely unavailable na ho jaaye.

**Junior:** Samjha sir, matlab fault tolerance ka matlab failures ko rokna nahi hai, balki failures ke bawajood system ko chalte rehna hai.

**Senior:** Bilkul sahi samjhe.

**ONE LINE STAFF ANSWER:** Fault tolerance high availability achieve karne ka real tareeka hai — single points of failure ko replication se eliminate karo, monitoring se faulty instances ko jaldi detect/isolate karo, aur restart ya rollback se jaldi recover karo, taaki failures user ko notice hi na hon.
