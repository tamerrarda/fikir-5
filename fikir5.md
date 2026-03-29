Fikir 5 — Trusted DAO Voting

Proje Adı: TrustVote

Hedef Track: World ID 4.0 ✦ World MiniKit ✦ Hedera B3 (SDK Only) — 3 track

Problem: Zincir üstü DAO oylamalarında bir cüzdan = bir oy mantığı bot saldırıları ve çoklu hesap manipülasyonuna karşı savunmasız; gerçek insan katılımını garanti eden bir mekanizma yok.

Çözüm: World App içinde çalışan bir mini DAO yönetişim uygulaması. Kullanıcı aktif bir oylamayı görüyor, World ID 4.0 ile "gerçek ve benzersiz insan" kanıtı sunuyor, backend proof'u doğruluyor ve Hedera Token Service'te tek kullanımlık oy tokenı mint ediliyor. Oy kullanıldıktan sonra token burn ediliyor. Tüm oy kayıtları Hedera Consensus Service'e immutable olarak yazılıyor ve Mirror Node üzerinden herkese açık audit trail olarak sunuluyor.

Teknik Stack:



@worldcoin/minikit-js MiniKit 2.0 (Verify + arayüz)

@worldcoin/idkit (World ID 4.0 backend proof verification)

@hashgraph/sdk (HTS: oy token mint/burn + HCS: audit log)

Next.js + TypeScript, Hono backend

Hedera Testnet



MVP — 3 kritik özellik:



MiniKit içinde oylama arayüzü + World ID 4.0 proof ile eligibility check (backend verify zorunlu — track şartı)

HTS'te tek kullanımlık oy tokenı mint → kullanım sonrası burn lifecycle

HCS'e her oy için immutable kayıt + Mirror Node'dan canlı sonuç okuma



Neden Kazanırız? Governance + sybil resistance jürinin anında kavradığı bir problem. World ID'nin "one-person-one-action" use-case'ini en temiz şekilde kullanan demo. HTS burn mekanizması + HCS audit trail + ZK proof üçgeni teknik derinlik gösteriyor. MiniKit sayesinde World App kullanıcı tabanına direkt dağıtım hikayesi var. 72 saatte polished demo çıkarmak için en güvenli fikir.

Track Uygunluk Notu: World ID 4.0 track şartı olan "proof validation must occur in a web backend or smart contract" backend verify ile tam karşılanıyor. MiniKit track şartı olan "integrate any MiniKit SDK commands" Verify komutu ile karşılanıyor, World Chain deploy zorunluluğu yok çünkü on-chain activity yok. Hedera B3 için HTS + HCS birlikte kullanılıyor, Solidity yok — tam SDK-only.

Yapılabilirlik: 9/10 — Tüm teknolojiler stabil. Solidity yok. Zor nokta: World ID 4.0 migration "Coming Soon" etiketiyle geliyor, 3.x ile başlayıp 4.0 üzerinde çalışmak gerekebilir, bunu baştan clarify etmek şart.

İşte TrustVote için detaylı jüri değerlendirmem:

1. Track Uyumu — 23/25
✅ Uygun: World ID 4.0'ın en kritik şartı olan "backend proof validation" adımının Hono backend ile tasarlanması tam on ikiden vurmuş. Hedera B3 (SDK Only) track'i için Solidity kullanmadan HTS (mint/burn) ve HCS (audit log) kombinasyonu yapmak, sponsorun tam olarak görmek istediği "native SDK gücü" vizyonuna uyuyor.
⚠️ Dikkat: World ID 4.0 migration sürecindeki belirsizlikler. Eğer API'lar beklediğiniz gibi çalışmazsa ve 3.x'e fallback yapmak zorunda kalırsanız, 4.0 spesifik ödül havuzundan puan kaybedebilirsiniz.
→ Neden bu puanı verdin: Sponsorların qualification şartlarını satır satır okumuş ve mimariyi onlara "evet, ürünümüzü tam da böyle kullanmalısınız" dedirtecek şekilde kurgulamış zeki bir ekip işi.

2. Teknik Fizibilite — 22/25
✅ Uygun: Solidity ve akıllı kontrat yazım sürecini (ve getirdiği test/deploy yükünü) tamamen devreden çıkararak 72 saati sadece entegrasyona ayırmak harika bir hackathon hayatta kalma stratejisi. Next.js + Hono + SDK üçlüsü oldukça stabil ve hızlı geliştirilebilir bir stack.
⚠️ Dikkat: Backend üzerinden Hedera ağına atılacak logların (HCS) ve mint/burn işlemlerinin asenkron doğası, MiniKit arayüzündeki kullanıcı deneyimini (loading stateleri) zorlayabilir.
→ Neden bu puanı verdin: Projenin kapsamı (MVP) 72 saat için son derece gerçekçi; teknik riskler minimumda tutulmuş ve pazar sabahı çalışan bir demo çıkarma ihtimali çok yüksek.

3. İnovasyon — 12/20
✅ Uygun: Bir EVM chain üzerinde standart bir DAO kontratı yazmak yerine, oylama verisini Hedera Consensus Service'e immutable log olarak basmak ve token lifecycle'ını HTS ile yönetmek taze bir mimari yaklaşım.
⚠️ Dikkat: "World ID ile botlara karşı korumalı oylama" fikri, Worldcoin track'lerinin tartışmasız en klişe fikridir. Her hackathonda bu konsepti deneyen en az 5 ekip olur.
❌ Uygun değil: Fikrin özü (problemin kendisi ve temel çözüm yolu) maalesef yeni bir şey söylemiyor; sadece kullanılan teknoloji kombinasyonu farklı.
→ Neden bu puanı verdin: Teknik mimarideki cross-chain/SDK inovasyonu çok başarılı olsa da, ürün vizyonu jüriye "vay canına, bunu daha önce kimse düşünmemişti" dedirtmiyor.

4. Pratiklik ve Fonksiyonellik — 17/20
✅ Uygun: Uygulamanın World App içinde bir MiniKit olarak çalışması, son kullanıcının cüzdan bağlama veya ağ değiştirme gibi dertlerini ortadan kaldırıyor. "One-click vote" deneyimi sunmaya çok müsait.
⚠️ Dikkat: Hedera tarafındaki transaction fee'lerini kim ödeyecek? Backend'in meta-transaction mantığıyla bu fee'leri karşılaması (sponsorlu işlemler) gerekecek, bunun demoda pürüzsüz çalışması lazım.
→ Neden bu puanı verdin: Hedef kitlenin (World App kullanıcıları) hemen bugün açıp kullanabileceği, başı ve sonu belli olan net bir ürün döngüsü var.

5. WOW Faktörü — 6/10
✅ Uygun: Mirror Node üzerinden oyların canlı ve şeffaf bir şekilde aktığını jüriye anlık olarak gösterebilmek güzel bir "aha!" anı yaratacaktır.
⚠️ Dikkat: Fikir çok "güvenli". Jüri olarak binlerce DAO tool'u gördüğümüz için, sunumun çok enerjik ve demonun kusursuz cilalanmış olması şart, aksi takdirde akılda kalıcılığı düşük olabilir.
→ Neden bu puanı verdin: Çok iyi çalışan ama heyecan katsayısı biraz düşük, "görev adamı" profiline sahip bir hackathon projesi.

TOPLAM PUAN: 80/100
Güçlü yönler:

Sponsor track gereksinimlerine (özellikle Hedera B3 ve World ID backend verification) kusursuz uyum.

Akıllı kontrat bağımlılığını kaldırarak 72 saatlik süreyi maksimize eden, çok gerçekçi bir MVP mimarisi.

World App ekosistemine MiniKit ile doğrudan entegre olarak sıfır pürüzlü kullanıcı deneyimi potansiyeli.

Kritik riskler:

Fikrin (Sybil-resistant DAO) aşırı popüler olması nedeniyle, diğer takımların kalabalığı arasında kaybolma riski.

World ID 4.0'ın beta sürümünden veya dokümantasyon eksikliğinden kaynaklanabilecek entegrasyon krizleri.

Jüri notu:
"Bu proje, ETHGlobal'de finale kalıp ana sahnede yarışacak o 'çılgın' fikirlerden biri değil; ancak sponsor ödüllerini (partner prizes) silip süpürmek için tasarlanmış kusursuz bir 'bounty hunter' makinesi. Mimari kararlar çok olgun, ne yapacaklarını biliyorlar. Sunum sırasında fikrin klişeliğini unutturmak için Hedera HCS/HTS hızını ve MiniKit'in UX pürüzsüzlüğünü gözümüze sokmaları gerekecek."