Fikir 5 — Trusted DAO Voting (Enhanced)

Proje Adı: TrustVote

Hedef Track: World ID 4.0 ✦ World MiniKit ✦ Hedera B3 (SDK Only) — 3 track

Problem: Zincir üstü DAO oylamalarında bir cüzdan = bir oy mantığı bot saldırıları ve çoklu hesap manipülasyonuna karşı savunmasız; gerçek insan katılımını garanti eden bir mekanizma yok. Ayrıca mevcut sistemlerde oyların sıralaması, şeffaflığı ve audit edilebilirliği güvenilir şekilde garanti edilemiyor.

Çözüm: World App içinde çalışan, sybil-resistant bir yönetişim protokolü. Kullanıcı aktif bir oylamayı görüyor, World ID 4.0 ile "gerçek ve benzersiz insan" kanıtı sunuyor, backend proof'u doğruluyor ve Hedera Token Service'te tek kullanımlık oy tokenı mint ediliyor. Oy kullanıldıktan sonra token burn ediliyor. Tüm oy kayıtları Hedera Consensus Service'e immutable ve sıralı (ordered) olarak yazılıyor ve Mirror Node üzerinden gerçek zamanlı (live) sonuçlar herkese açık olarak sunuluyor.

Ek olarak sistem, kullanıcıların geçmiş katılımına dayalı basit bir reputation score üretir ve bu skor oylama ağırlığını etkileyebilir. Birden fazla DAO oluşturulabilir ve her DAO kendi HCS topic'i üzerinden bağımsız olarak çalışır.

Teknik Stack:

@worldcoin/minikit-js MiniKit 2.0 (Verify + arayüz + native UX akışı)

@worldcoin/idkit (World ID 4.0 backend proof verification)

@hashgraph/sdk (HTS: oy token mint/burn + HCS: ordered audit log & state tracking)

Next.js + TypeScript, Hono backend

Hedera Testnet + Mirror Node (real-time data read)

MVP — Genişletilmiş kritik özellikler:

MiniKit içinde frictionless oylama akışı (1-tap verify + 1-tap vote) + World ID 4.0 proof ile eligibility check (backend verify zorunlu — track şartı)

HTS'te tek kullanımlık oy tokenı mint → kullanım sonrası burn lifecycle (tam token lifecycle)

HCS'e her oy için immutable ve ordered kayıt + Mirror Node'dan live sonuç akışı (real-time dashboard)

Multi-DAO desteği: Her oylama ayrı HCS topic ile yönetilir (scalable governance yapısı)

Basit reputation sistemi: Kullanıcının geçmiş oylama katılımına göre oy ağırlığı (identity-based governance)

Gasless UX: Tüm Hedera işlem ücretleri backend tarafından karşılanır (kullanıcı için sıfır friction)

Sybil attack demo: Fake hesaplar ile oy kullanılamadığı, yalnızca World ID doğrulanmış kullanıcıların katılabildiği canlı gösterim

Neden Kazanırız? Governance + sybil resistance jürinin anında kavradığı bir problem. Ancak bu proje yalnızca bir DAO voting app değil; gerçek insan kimliği üzerine inşa edilmiş bir governance altyapısıdır. World ID ile one-person-one-vote garantilenirken, Hedera Consensus Service sayesinde oyların sıralaması manipüle edilemez ve tüm süreç audit edilebilir hale gelir. MiniKit entegrasyonu sayesinde bu sistem doğrudan World App kullanıcılarına frictionless şekilde sunulur. Live sonuçlar ve sybil attack demo, jüriye sistemin gerçekliğini anlık olarak gösterir.

Track Uygunluk Notu: World ID 4.0 track şartı olan "proof validation must occur in a web backend or smart contract" backend verify ile tam karşılanıyor. Ayrıca World ID sadece giriş değil, governance primitive olarak kullanılıyor (proposal, voting, reputation). MiniKit track şartı olan SDK entegrasyonu Verify akışı ve native UX ile güçlü şekilde karşılanıyor. Hedera B3 için HTS + HCS birlikte kullanılıyor, ayrıca HCS ordering ve audit özellikleri aktif olarak ürünün core mekanizmasına entegre edilmiş durumda; Solidity yok — tam SDK-only ve advanced usage.

Yapılabilirlik: 9.5/10 — Core sistem oldukça stabil ve hızlı geliştirilebilir. Eklenen feature’lar (live results, multi-DAO, reputation, gasless UX) düşük-orta kompleksitede olup 72 saat içinde uygulanabilir. Yüksek riskli feature’lardan (delegation, quadratic voting) bilinçli olarak kaçınılmıştır.

İşte TrustVote için güncellenmiş jüri değerlendirmem:

1. Track Uyumu — 24/25
✅ Uygun: World ID'nin backend verification ve identity-based kullanım şartları güçlü şekilde karşılanıyor. Hedera tarafında HTS + HCS sadece kullanılmakla kalmayıp ürünün core logic'ine entegre edilmiş. MiniKit entegrasyonu gerçek bir ürün hissi veriyor.
⚠️ Dikkat: World ID 4.0 migration sürecindeki olası API değişiklikleri.

2. Teknik Fizibilite — 23/25
✅ Uygun: Stack sade ve hızlı geliştirilebilir. Live data akışı ve multi-DAO yapısı iyi planlanmış.
⚠️ Dikkat: HCS + Mirror Node veri senkronizasyonunda latency yönetimi.

3. İnovasyon — 16/20
✅ Uygun: DAO voting konsepti üzerine reputation, ordered consensus ve live audit layer eklenerek daha güçlü bir governance primitive'e dönüştürülmüş.
⚠️ Dikkat: Temel fikir hâlâ bilindik, ancak execution fark yaratıyor.

4. Pratiklik ve Fonksiyonellik — 18/20
✅ Uygun: Gasless UX ve MiniKit entegrasyonu sayesinde kullanıcı deneyimi çok güçlü. Multi-DAO desteği ürünü genişletilebilir kılıyor.

5. WOW Faktörü — 9/10
✅ Uygun: Live voting dashboard + sybil attack demo + real-time Hedera data akışı güçlü bir sahne etkisi yaratır.

TOPLAM PUAN: 90/100

Güçlü yönler:

Track gereksinimlerine derinlemesine uyum ve sponsor teknolojilerinin doğru konumlandırılması

Gerçek zamanlı veri akışı ve audit edilebilir governance yapısı

MiniKit sayesinde frictionless ve dağıtıma hazır kullanıcı deneyimi

Kritik riskler:

World ID 4.0 API belirsizlikleri

Demo sırasında live data akışında yaşanabilecek gecikmeler

Jüri notu:
"Bu proje klasik bir DAO voting aracından çok daha fazlası; gerçek insan kimliği üzerine kurulu, şeffaf ve manipülasyona kapalı bir governance altyapısı sunuyor. Hedera'nın consensus avantajını ve World ID'nin sybil resistance gücünü doğru şekilde birleştirmiş. Demo akıcı olduğu sürece partner ödüllerinde çok güçlü bir aday."

Eklenen Geliştirmeler (Feature Set)

CORE EXTENSIONS

Live Results (Real-time Voting Dashboard)
Oylar anlık olarak güncellenir ve kullanıcıya gerçek zamanlı sonuçlar sunulur. Veriler Hedera Mirror Node üzerinden çekilir ve frontend’de yüzdelik dağılım, toplam oy sayısı ve grafikler ile gösterilir. Bu özellik Hedera Consensus Service’in aktif kullanımını görünür hale getirir.

Multi-DAO Support
Sistem tek bir oylama ile sınırlı değildir. Birden fazla DAO veya oylama oluşturulabilir. Her DAO ayrı bir Hedera Consensus Service (HCS) topic’i üzerinden yönetilir. Bu sayede proje bir uygulamadan ziyade ölçeklenebilir bir governance platformuna dönüşür.

Reputation-based Voting (Lite)
Kullanıcıların geçmiş oylama katılımına göre basit bir reputation skoru hesaplanır. Bu skor, oylama sırasında oy ağırlığını etkileyebilir. Böylece World ID sadece doğrulama değil, aynı zamanda identity tabanlı bir yönetişim katmanı haline gelir.


HEDERA-ODAKLI GELİŞTİRMELER

Ordered Vote Logging (HCS Advantage)
Tüm oylar Hedera Consensus Service üzerinde sıralı (ordered) ve değiştirilemez şekilde kaydedilir. Bu, oylama sürecinde manipülasyon ihtimalini ortadan kaldırır ve sistemin güvenilirliğini artırır.

Full Token Lifecycle (HTS)
Oy mekanizması Hedera Token Service üzerinden yönetilir. Kullanıcı doğrulandıktan sonra tek kullanımlık oy tokenı mint edilir, oy kullanıldıktan sonra bu token burn edilir. Bu lifecycle, HTS’in gerçek bir kullanım senaryosu ile entegre edildiğini gösterir.

Real-time Audit Layer
Tüm oylama verileri herkese açık ve şeffaf şekilde sunulur. Hedera Mirror Node üzerinden veriler okunarak kullanıcıya gerçek zamanlı audit imkanı sağlanır.


MINIKIT UX GELİŞTİRMELERİ

Frictionless Voting Flow
Kullanıcı deneyimi maksimum sadelik üzerine kuruludur. Kullanıcı tek tık ile World ID doğrulaması yapar ve tek tık ile oy kullanır. Cüzdan bağlantısı veya ağ seçimi gibi ekstra adımlar yoktur.

Gasless UX
Tüm Hedera işlem ücretleri backend tarafından karşılanır. Kullanıcı hiçbir işlem ücreti ödemez. Bu, özellikle World App kullanıcıları için frictionless bir deneyim sağlar.

Vote History / Feed
Kullanıcılar geçmişte katıldıkları oylamaları ve DAO’ları görüntüleyebilir. Bu özellik uygulamayı tek seferlik kullanım yerine sürekli etkileşimli bir ürüne dönüştürür.


WORLD ID GÜÇLENDİRMELERİ

Human-Gated Actions
World ID doğrulaması sadece oy kullanmak için değil, aynı zamanda proposal oluşturma ve DAO’ya katılım gibi kritik aksiyonlar için de zorunlu hale getirilir. Bu sayede sistem tamamen gerçek insan katılımına dayanır.

Sybil Resistance Demo
Demo sırasında sahte (fake) hesaplarla oy kullanılamadığı, yalnızca World ID doğrulaması yapılmış kullanıcıların oylamaya katılabildiği canlı olarak gösterilir. Bu, projenin değer önerisini net bir şekilde ortaya koyar.

Reusable Identity Session (Opsiyonel)
Kullanıcıların tekrar tekrar doğrulama yapmasına gerek kalmaz. Oturum bazlı identity kullanımı ile kullanıcı deneyimi iyileştirilir.


DEMO / WOW FEATURE’LAR

Live Voting Screen
Oylama sırasında sonuçlar anlık olarak değişir ve jüriye canlı veri akışı gösterilir. Bu, sistemin gerçekten çalıştığını kanıtlayan en güçlü demo anıdır.

%100 Human Votes Göstergesi
Sistemde kullanılan oyların tamamının doğrulanmış gerçek kullanıcılardan geldiği yüzdesel olarak gösterilir.

Global Participation Hissi (Opsiyonel)
Farklı kullanıcıların katılımı simüle edilerek global bir oylama ortamı hissi yaratılır.