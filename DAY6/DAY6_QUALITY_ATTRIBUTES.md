**Q: Performance kya hai?**

A: Matlab system kitni fast kaam karta hai aur kitna kaam ek time mein handle kar sakta hai — do cheezein: response time (speed) aur throughput (volume).

**Example:** Amazon pe search karte ho aur 100ms mein products dikh jaate hain — ye response time hai. Ek logging system jo lakhon logs per second process karta hai — ye throughput hai.

**Kaise measure karte hain (industry mein):**
1. **Response time percentiles (p50, p95, p99)** — milliseconds mein, average nahi balki percentile se measure karte hain taaki slow users bhi dikhein.
2. **Throughput** — requests per second (RPS) ya data processed per second (MB/s).
3. **Tools** — New Relic, Datadog, Grafana se ye sab track karte hain real-time mein.

---

**Q: Scalability kya hai?**

A: Matlab system apna kaam badhta hua load easily aur cost-effectively handle kar sake, resources add karke.

**Example:** Diwali sale pe Flipkart ka traffic 10x badh jaata hai, aur company sirf aur servers add karke usko handle kar leti hai, bina system tode.

**Kaise measure karte hain (industry mein):**
1. **Load testing** — tools jaise JMeter, Gatling, k6 se artificial traffic generate karke dekhte hain system kitna load handle kar sakta hai.
2. **Resource vs Capacity ratio** — resources double karne pe capacity kitni badhi (linear scalability best case hai).
3. **Degradation point** — kis load pe system ka performance kharab hona shuru hota hai, wo point identify karte hain.

---

**Q: Availability kya hai?**

A: Matlab system kitne percent time actually up aur usable rehta hai, saal bhar mein.

**Example:** Netflix agar 99.9% available hai, matlab saal mein sirf kuch ghanton ke liye hi down hota hai, baaki hamesha chalu rehta hai.

**Kaise measure karte hain (industry mein):**
1. **Uptime formula** — (Total time − Downtime) / Total time × 100.
2. **"Nines" terminology** — 99.9% ko "three nines" bolte hain, 99.99% ko "four nines," jitne zyada nines utni behtar availability.
3. **Tools** — Pingdom, UptimeRobot, StatusPage se continuously monitor karte hain.

---

**Q: Portability kya hai?**

A: Matlab tumhara system/code ek jagah (ek OS, ek cloud, ek database) pe likha ho, aur usko kisi doosri jagah shift karne mein bahut kam ya zero rewriting lage.

**Example:** Tumne apna app Java mein likha aur Windows pe test kiya. Ab wahi app bina code change kiye Linux server pe ya AWS pe chal jaaye — ye portability hai. Iske opposite: agar app sirf ek specific OS ya ek specific cloud pe hi chal sakta hai, toh wo "not portable" hai.

**Kaise measure karte hain (industry mein):**
1. **Migration effort** — naye environment pe move karne mein kitne files/lines of code change karne pade (jitna kam, utna zyada portable).
2. **Containerization check** — agar app Docker container mein pack hai, toh wo almost kahin bhi chal sakta hai — ye ek direct portability signal hai.
3. **Dependency count** — kitni platform-specific libraries use ki hain (jaise sirf Windows API calls) — jitni kam, utni zyada portability.

---

**Q: Security kya hai?**

A: Matlab system ko hackers, data chori, aur galat access se bachana — sirf "password lagana" nahi, balki poora protection layer.

**Example:** Bank ka app tumhara password kabhi plain text mein store nahi karta (encrypt karta hai), login pe OTP maangta hai, aur agar koi 5 baar galat password daale toh account lock kar deta hai.

**Kaise measure karte hain (industry mein):**
1. **Number of open vulnerabilities** — security scan tools (jaise OWASP ZAP, Burp Suite) chalake dekhte hain kitne security holes mile.
2. **Time to patch** — koi vulnerability milne ke baad usko fix karne mein kitna time laga (jitna kam, utna better).
3. **Penetration test results** — ethical hackers hire karke system attack karwate hain, aur dekhte hain kitni cheezein break ho jaati hain.
4. **Number of security incidents/breaches** — kitni baar actual attack successful hua, over time.

---

**Q: Observability kya hai?**

A: Matlab jab system mein kuch galat ho, tumhe turant pata chal jaaye kya hua aur kaha hua — bina system ko chhede.

**Example:** Ek server slow ho raha hai. Engineer apna dashboard (Grafana) kholta hai aur turant dekh leta hai ki problem ek specific database query se aa rahi hai, na ki poore system mein dhoondhna padta hai.

**Kaise measure karte hain (industry mein):**
1. **MTTD (Mean Time To Detect)** — issue hone se lekar pata chalne tak kitna time laga.
2. **MTTR (Mean Time To Resolve)** — pata chalne se lekar fix hone tak kitna time laga.
3. **Log/metric coverage** — system ke kitne parts se logs aur metrics actually collect ho rahe hain.
4. **Alert accuracy** — kitne alerts false alarms nikalte hain vs genuine issues (kam false alarms = better observability setup).

---

**Q: Consistency kya hai?**

A: Matlab system ke alag alag jagah (do servers, do databases) pe same data ek hi tarah dikhna chahiye — koi ek jagah purana data na dikhe.

**Example:** Tumne app pe ₹500 transfer kiya. Turant tumhara balance app pe bhi update ho aur website pe bhi — dono jagah same number dikhna chahiye, ek jagah purana balance nahi rehna chahiye.

**Kaise measure karte hain (industry mein):**
1. **Replication lag** — ek server pe data update hone se doosre server tak pahunchne mein kitna time (milliseconds) lagta hai.
2. **Stale read percentage** — kitne % requests ko purana (outdated) data mila, total requests mein se.
3. **Conflict rate** — distributed systems mein kitni baar do updates aapas mein clash karte hain (conflicting writes).

---

**Q: Efficiency kya hai?**

A: Matlab system apna kaam kam se kam resources (CPU, memory, battery, network data) use karke kare — waste na kare.

**Example:** Ek chat app jo message bhejne ke liye bahut kam data aur battery use kare (jaise WhatsApp), vs ek app jo simple text message bhejne ke liye bhi phone garam kar de aur bahut data khaye.

**Kaise measure karte hain (industry mein):**
1. **CPU/Memory utilization** — server ek request process karne mein kitna CPU/RAM use karta hai.
2. **Cost per request** — cloud bill ke hisaab se, ek request serve karne mein company ko kitna paisa lagta hai.
3. **Battery/data usage (mobile apps ke liye)** — app profiling tools se measure karte hain.
4. **Energy consumption per operation** — bade data centers mein power usage bhi track hota hai (green computing angle se).

---

**Q: Usability kya hai?**

A: Matlab ek naya user, bina kisi training ya manual padhe, system ko easily use kar paaye.

**Example:** WhatsApp kholte hi pata chal jaata hai kaise message bhejna hai — koi tutorial nahi chahiye. Compare karo ek purane enterprise software se jisme button dhoondhne ke liye hi 10 minute lag jaate hain.

**Kaise measure karte hain (industry mein):**
1. **Task Success Rate** — real users ko ek task diya jaata hai (jaise "order place karo"), aur dekha jaata hai kitne % bina help ke complete kar paate hain.
2. **Time on Task** — task complete karne mein average kitna time laga.
3. **NPS/User satisfaction survey** — users se directly rating li jaati hai.
4. **Error rate** — kitni baar users galat button dabate hain ya confuse hote hain use karte waqt.

---

**Q: Testability kya hai?**

A: Matlab system ke individual parts ko alag alag, chhote-chhote pieces mein test kar sakte ho — bina poora system chalaye.

**Example:** Agar "payment module" ko akela test kiya jaa sakta hai (fake/mock data se), bina poori website chalaye — ye high testability hai. Agar test karne ke liye har baar poora system start karna pade, ye low testability hai.

**Kaise measure karte hain (industry mein):**
1. **Code coverage %** — tools jaise JaCoCo, Istanbul se pata chalta hai kitna % code automated tests se cover hota hai.
2. **Test execution time** — poori test suite chalane mein kitna time lagta hai.
3. **Number of flaky tests** — kitne tests kabhi pass kabhi fail hote hain bina code change ke (jitne kam, utni behtar testability).

---

**Q: Deployability kya hai?**

A: Matlab naya code/feature production (real users tak) mein bhejna kitna easy aur safe hai.

**Example:** Ek team din mein 5 baar bhi naya code release kar sakti hai, bina system down kiye — ye high deployability hai. Compare karo ek team se jo saal mein sirf 2 baar release karti hai kyunki har release mein risk bahut zyada hota hai.

**Kaise measure karte hain (industry mein):**
1. **Deployment Frequency** — kitni baar per day/week naya code release hota hai (ye ek official "DORA metric" hai, industry standard).
2. **Change Failure Rate** — kitne % deployments ke baad kuch toot jaata hai ya rollback karna padta hai.
3. **Lead Time for Changes** — code likhne se lekar production mein live hone tak kitna time lagta hai.
4. **Mean Time to Recovery (rollback time)** — agar deployment fail ho jaaye, purani working state pe wapas aane mein kitna time lagta hai.

---

**Q: Maintainability kya hai?**

A: Matlab purane code mein naya feature add karna ya bug fix karna kitna easy hai, bina poora system samjhe ya tode.

**Example:** Ek clean, well-organized codebase mein naya developer 2 din mein ek bug fix kar leta hai. Ek messy, purani codebase mein wahi bug fix karne mein 2 hafte lag jaate hain kyunki code samajhna hi mushkil hai.

**Kaise measure karte hain (industry mein):**
1. **Time to fix a bug** — average kitna time lagta hai ek bug report se lekar fix deploy hone tak.
2. **Cyclomatic Complexity** — code kitna "tangled"/complex hai, tool se measure hota hai (jitna kam number, utna simple aur maintainable code).
3. **Onboarding time** — naye developer ko team mein pehla contribution karne mein kitna time lagta hai.
4. **Technical debt ratio** — tools jaise SonarQube se measure karte hain kitna "shortcut" code hai jo future mein problem create karega.

**ONE LINE STAFF ANSWER:** Har quality attribute ka apna concrete number/metric hota hai jisse use industry mein measure karte hain — performance ke liye percentile latency, availability ke liye uptime %, security ke liye open vulnerabilities, deployability ke liye deployment frequency, aur maintainability ke liye bug-fix time — "acha lagta hai" bolna kaafi nahi, actual data dikhana padta hai.
