**Junior:** Sir, API design cover kar liya humne. Ab specific API types shuru karte hain?

**Senior:** Haan, pehla type hai **RPC — Remote Procedure Call**. Pehle samajhte hain ye hai kya.

**Junior:** RPC kya hota hai?

**Senior:** RPC matlab client application ki ability jisse wo ek remote server pe koi function ya subroutine execute kar sake. Par isme special baat ye hai — code likhte waqt, remote method call bilkul aisa dikhta hai jaisa ek normal local method call. Isi feature ko **local transparency** bolte hain.

**Junior:** Matlab developer ko pata hi nahi chalta ki wo local method call kar raha hai ya remote?

**Senior:** Exactly, code ke perspective se dono almost same dikhte hain. Aur ek aur feature hota hai — zyadatar RPC frameworks **multiple programming languages** support karte hain, taaki alag alag languages mein likhi applications bhi aapas mein baat kar sakein.

**Junior:** Ye kaam kaise karta hai internally?

**Senior:** Chalo step by step dekhte hain:
1. API aur uske data types ek special **Interface Description Language (IDL)** mein define kiye jaate hain — ye ek schema hoti hai client-server communication ki.
2. Ek compiler ya code generator tool (RPC framework ka part) is definition se do implementations auto-generate karta hai — **server stub** aur **client stub**.
3. Custom object types jo IDL mein define kiye the, wo classes ya structs mein compile ho jaate hain — inhe **DTOs (Data Transfer Objects)** bolte hain.

**Junior:** Aur runtime pe actually kya hota hai jab client call karta hai?

**Senior:** Runtime pe process kuch aisa hota hai:
1. Client method call karta hai → client stub data ko encode karta hai (isko **serialization/marshalling** bolte hain).
2. Client stub network ke through server stub ko data bhejta hai.
3. Server stub data ko decode (**deserialize**) karta hai aur actual method implementation ko invoke karta hai server pe.
4. Server operation complete karta hai → result server stub ko wapas jaata hai → server stub response serialize karke client ko bhej deta hai.
5. Client stub response ko deserialize karke caller ko wapas de deta hai, bilkul ek normal local method ke return value ki tarah.

**Junior:** Isse fayda kya hota hai?

**Senior:** Ek baar API IDL ke through define aur publish ho jaaye, toh client aur server **completely decoupled** ho jaate hain. Server team apna server stub generate kar sakti hai, aur jab bhi koi naya client integrate karna chahe, unhe sirf published API definition chahiye apna client stub generate karne ke liye — server team se directly coordinate karne ki zaroorat nahi.

**Junior:** Aur multiple languages support karne se kya benefit hai?

**Senior:** Agar framework multiple languages support karta hai, toh server team apni language mein likh sakti hai, aur client apni favourite language choose kar sakta hai — dono independent rehte hain language choice mein.

**Junior:** Samjha sir. Ab RPC ke benefits kya hain?

**Senior:** Kuch important benefits hain:
1. Convenience — client developer sirf normal method calls karta hai, sab networking details abstract ho jaati hain.
2. Communication failures bhi simple errors/exceptions ki tarah dikhte hain, jaise normal method calls mein hota hai.
3. Local transparency — remote calls, local calls jaisi hi feel hoti hain.

**Junior:** Aur drawbacks?

**Senior:** Do main drawbacks hain:
1. Slowness
2. Unreliability

Chalo dono ko detail mein dekhte hain.

**Junior:** Slowness wala kya issue hai?

**Senior:** Remote calls local method calls se kaafi slow hote hain — par code mein dono almost same dikhte hain, isliye developer ko surprise mil sakta hai performance bottlenecks ke roop mein, kyunki wo assume kar leta hai ki ye bhi fast hoga jaise local method.

**Junior:** Iska solution kya hai?

**Senior:** Wahi jo humne pichli lecture mein seekha tha — slow methods ke liye **asynchronous versions** provide karo.

**Junior:** Ab unreliability wala issue samjhaiye.

**Senior:** Client remotely chal raha hota hai — kabhi kabhi ek doosri company ke system pe — aur network pe depend karta hai. Isse uncertainty aa jaati hai. Ek classic example dekhte hain.

**Junior:** Bataiye example.

**Senior:** Socho tum ek credit card company ho, aur ek online store client tumhare "debit account" API ko call kar raha hai. Agar call fail ho jaaye ya timeout ho jaaye:
1. Retry karo — toh risk hai user se double charge ho jaaye.
2. Retry na karo — toh risk hai user se charge hi na ho.

**Junior:** Client ko pata kaise chalega actually kya hua?

**Senior:** Yahi problem hai — client ko koi tareeka nahi hai ye pata karne ka ki server ne request actually process kar li thi aur sirf acknowledgment lost ho gaya, ya server crash ho gaya request receive karne se pehle hi.

**Junior:** Toh iska koi solution hai?

**Senior:** Poori tarah se solve toh nahi kar sakte, par mitigate kar sakte hain — wahi practice jo humne pehle seekhi thi, operations ko **idempotent** banao jahan possible ho. Isse safe retries possible ho jaate hain.

**Junior:** Samjha sir. Ab batao RPC use kab karna chahiye aur kab nahi?

**Senior:** RPC ka best use case hai:
1. Do backend systems ke beech communication — sabse common use case.
2. Ek large scale system ke internal components ke beech communication.
3. Jab tum network communication ko poori tarah abstract karna chahte ho aur sirf "actions" pe focus karna chahte ho jo client server pe perform karna chahta hai.

**Junior:** Aur frontend clients ke liye RPC kaisa hai?

**Senior:** Web browsers jaise frontend clients ke liye RPC kam common hai — kuch frameworks support karte hain, par typical use case nahi hai.

**Junior:** Aur kab RPC avoid karna chahiye?

**Senior:** Do situations hain:
1. Agar tumhe directly HTTP cookies ya headers ka fayda uthana hai — RPC network ko abstract kar deta hai, toh ye use case fit nahi baithta.
2. Agar tumhara API zyada **data-centric** hai aur sirf simple CRUD operations chahiye — RPC actions ke around bana hota hai (har action ek naya method, apne naam aur signature ke saath), jo aisi situations ke liye best fit nahi hai.

**Junior:** Toh data-centric APIs ke liye koi doosra style hai?

**Senior:** Haan, wo hum agli lecture mein cover karenge.

**Junior:** Samjha sir, matlab RPC actions-based, backend-to-backend communication ke liye best hai, par thoda slow aur unreliable ho sakta hai jisse handle karna padta hai async aur idempotency se.

**Senior:** Bilkul sahi samjhe.

**ONE LINE STAFF ANSWER:** RPC client ko remote method call ko bilkul local method call jaisa feel karwata hai (local transparency), IDL se auto-generated client/server stubs use karke — ye backend-to-backend aur internal communication ke liye best hai, par slowness aur unreliability jaise drawbacks aate hain jinhe async operations aur idempotency se mitigate karte hain.
