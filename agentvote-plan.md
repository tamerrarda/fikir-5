# AgentVote — Hackathon Planlama Dökümanı

---

## Track Analizi

### World Track

World'ün en yüksek ödüllü track'i **AgentKit ($8,000)**. Şart: World ID'nin safety/fairness/trust sağladığı agentic deneyimler. Sadece World ID veya MiniKit kullanan projeler bu track için **disqualify** ediliyor.

| Track             | Şart                                                                | Ödül   |
| ----------------- | ------------------------------------------------------------------- | ------ |
| World AgentKit    | Human-backed agent; World ID safety guarantee; AgentKit SDK zorunlu | $8,000 |
| World ID 4.0      | Backend proof validation; nullifier ile uniqueness; real constraint | $8,000 |
| World MiniKit 2.0 | Mini App + Verify komutu; backend proof validation                  | $4,000 |

### Hedera Track

| Track                 | Şart                                                       | Ödül             |
| --------------------- | ---------------------------------------------------------- | ---------------- |
| AI & Agentic Payments | Autonomous payment yapan AI agent; Hedera Agent Kit / x402 | $6,000 (2 takım) |
| No Solidity           | HTS + HCS + Mirror Node; sıfır Solidity                    | $3,000 (3 takım) |
| Tokenization          | HTS ile real-world asset tokenizasyonu                     | $2,500 (2 takım) |

---

## Revize Proje: AgentVote

**Proje Adı:** AgentVote  
**Hedef Track:** World AgentKit ✦ World ID 4.0 ✦ World MiniKit 2.0 ✦ Hedera AI & Agentic Payments ✦ Hedera No Solidity

### Problem

Zincir üstü DAO oylamalarında iki köklü sorun: (1) bir cüzdan = bir oy mantığı Sybil saldırılarına karşı savunmasız, (2) token holder'ların büyük çoğunluğu her proposal'ı takip edecek kapasitede değil, katılım oranları kronik olarak düşük. Mevcut çözümler bu iki sorunu ayrı ayrı ele alıyor; hiçbiri insan kimliği garantisi ile akıllı delegasyonu aynı anda sunmuyor.

### Çözüm

World App içinde çalışan bir agentic DAO yönetişim uygulaması. Kullanıcı World ID 4.0 ile kendini kanıtlanmış bir insan olarak doğruladıktan sonra bir AI agent'a oy delegasyonu veriyor. Agent proposal'ları analiz ediyor, bir analiz servisine x402 protokolüyle HBAR mikro-ödeme yapıyor ve insan adına otonom oy kullanıyor. Her adım Hedera Consensus Service'e immutable log olarak yazılıyor.

### Teknik Stack

```
@worldcoin/minikit-js         MiniKit 2.0 — delegasyon ve oylama arayüzü
@worldcoin/idkit              World ID 4.0 — backend ZK proof verification
@world/agent-kit              AgentKit — human-backed agent orchestration
@hashgraph/sdk                HTS: oy token mint/burn + HCS: audit log
@hashgraph/hedera-agent-kit   Hedera Agent Kit — otonom payment flows
x402 middleware               Pay-per-request analiz servisi
Next.js + TypeScript          Frontend
Hono                          Backend
Hedera Testnet                Tüm on-chain işlemler
```

### MVP — 4 Kritik Özellik

**1. Human-Backed Delegation**  
Kullanıcı MiniKit arayüzünden World ID 4.0 ile ZK proof oluşturur. Hono backend proof'u IDKit ile doğrular. AgentKit katmanı bu kullanıcıyı "human-verified" olarak imzalar ve agent'a delegasyon yetkisi tanımlar. World ID nullifier hash'i tekrar kullanımı önler.

**2. Autonomous Vote Agent**  
AgentKit + Hedera Agent Kit üzerinde çalışan agent: proposal okur → x402 ile analiz servisine ödeme yapar → analizi alır → karar verir → HTS mint → oy kullanır → HTS burn. İnsan müdahalesi gerektirmez.

**3. Agent-to-Agent Payment Flow**  
Analiz servisi x402 pay-per-request modeliyle çalışır. Vote Agent her sorgu için HBAR mikro-ödeme yapar. Mirror Node üzerinden canlı izlenebilir.

**4. Verifiable Audit Trail**  
Her agent eylemi HCS'e immutable log olarak yazılır: delegasyon, analiz sorgusu, ödeme, karar, mint, oy, burn. Mirror Node REST API üzerinden herkese açık.

### Sistem Akışı

```
Kullanıcı (World App)
    │
    ├─ MiniKit: Proposal listesi görüntüle
    ├─ MiniKit: World ID 4.0 proof oluştur
    │
    ▼
Hono Backend
    ├─ IDKit: ZK proof verify        ← World ID 4.0 track şartı
    ├─ AgentKit: Human-verified imza ← AgentKit track şartı
    └─ Agent'a delegasyon ilet
    │
    ▼
AgentKit Orchestrator
    ├─ Proposal Agent:  On-chain veri oku
    ├─ Payment Agent:   x402 → HBAR mikro-ödeme
    ├─ Vote Agent:      Karar → HTS mint → Oy → HTS burn
    └─ Audit Agent:     Her adımı HCS'e yaz
    │
    ▼
Hedera Testnet
    ├─ HTS:         Token mint → burn lifecycle
    ├─ HCS:         Immutable audit log
    └─ Mirror Node: Herkese açık sonuç okuma
```

### Track Uygunluk Tablosu

| Track                        | Karşılanan Şart                                                | Ödül Havuzu |
| ---------------------------- | -------------------------------------------------------------- | ----------- |
| World AgentKit               | Human-backed vote agent; AgentKit SDK; World ID safety         | $8,000      |
| World ID 4.0                 | Backend proof validation (Hono + IDKit); nullifier uniqueness  | $8,000      |
| World MiniKit 2.0            | Mini App + Verify; delegasyon UI; backend proof validation     | $4,000      |
| Hedera AI & Agentic Payments | x402 agent-to-agent payment; Hedera Agent Kit; autonomous HBAR | $6,000      |
| Hedera No Solidity           | HTS + HCS + Mirror Node; sıfır Solidity                        | $3,000      |
| **Toplam**                   |                                                                | **$29,000** |

### TrustVote → AgentVote Karşılaştırması

|                    | TrustVote             | AgentVote                        |
| ------------------ | --------------------- | -------------------------------- |
| Track sayısı       | 3                     | 5                                |
| Max prize pool     | ~$15,000              | ~$29,000                         |
| World AgentKit     | ❌                    | ✅                               |
| Hedera AI Payments | ❌                    | ✅                               |
| x402 payment flow  | ❌                    | ✅                               |
| Oy mekanizması     | Kullanıcı oy kullanır | Agent otonom oy kullanır         |
| Narratif           | DAO oylama tool'u     | Agentic economy kimlik altyapısı |

---

## Yapılabilirlik Değerlendirmesi

**Genel puan:** 5.5/10 — Riskli ama yönetilebilir (kapsam kısılırsa)

### Temel Sorun

|                            | Gereken  | Gerçekçi tahmin |
| -------------------------- | -------- | --------------- |
| Çalışma saati (72h - uyku) | 54 saat  | 54 saat         |
| Toplam iş yükü (5 track)   | ~39 saat | ~64 saat        |
| Açık                       | —        | **+25 saat 🔴** |

Sıfır deneyimle 5 track'in entegrasyon yükü projeyi bitiremeden zamanı tüketir.  
**Öneri: Önce 2 track'i garantile, sonra ekstra track dene.**

### En Büyük 3 Risk

**Risk 1 — AgentKit dokümantasyonu ince.**  
"Human-backed agent" akışını World ID proof'uyla birleştiren end-to-end örnek yok. Tahmin: 4 saatlik iş 10-12 saate uzayabilir.

**Risk 2 — x402 + Hedera Agent Kit testnet stabilitesi.**  
x402 henüz tam üretim olgunluğunda değil. Transaction timeout veya fee sorunlarıyla karşılaşma ihtimali yüksek.

**Risk 3 — Demo kırılganlığı.**  
5 farklı sistemin sırayla çalışması gerekiyor. Tek bir timeout demo'yu mahvedebilir. Fallback video zorunlu.

---

## 72 Saatlik Plan

> **Strateji:** Core First, Agent Later — ilk 28 saatte 2 track'i garantile, sonra ekstra katmanları ekle.

### Özet Tablo

|                                   | Değer                     |
| --------------------------------- | ------------------------- |
| Toplam çalışma saati (72h - uyku) | 54 saat                   |
| Hedef track sayısı                | 2 (garantili) + 3 (bonus) |
| Ekip                              | 3 kişi                    |
| Yapılabilirlik                    | ⚠️ 5.5 / 10               |

### Faz Özeti

| Faz                       | Zaman             | Toplam Süre | Durum                   |
| ------------------------- | ----------------- | ----------- | ----------------------- |
| Faz 1 — Kurulum & Öğrenme | Gün 1, saat 0–16  | 16 saat     | 🟣 Başlangıç            |
| Faz 2 — Core MVP          | Gün 2, saat 16–36 | 20 saat     | 🟢 Kritik               |
| Faz 3 — Stabilizasyon     | Gün 3, saat 36–48 | 12 saat     | 🟠 Risk yönetimi        |
| Faz 4 — Sunum             | Gün 3, saat 48–54 | 6 saat      | 🔴 Pazarlık kabul etmez |

### Faz 1 — Kurulum & Öğrenme (Gün 1, saat 0–16)

| Görev                                   | Atanan   | Süre | Risk       |
| --------------------------------------- | -------- | ---- | ---------- |
| Next.js + TypeScript + Hono boilerplate | Kişi 1   | 4 sa | Düşük      |
| World ID 4.0 dokümantasyon okuma        | Kişi 2   | 3 sa | Orta       |
| Hedera testnet hesabı + SDK ilk adımlar | Kişi 3   | 3 sa | Orta       |
| MiniKit hello world                     | Tüm ekip | 3 sa | Orta       |
| İlk entegrasyon senkronizasyonu         | Tüm ekip | 3 sa | **Kritik** |

### Faz 2 — Core MVP (Gün 2, saat 16–36)

| Görev                             | Atanan | Süre | Risk       |
| --------------------------------- | ------ | ---- | ---------- |
| World ID 4.0 backend proof verify | Kişi 2 | 6 sa | **Kritik** |
| HTS token mint → burn lifecycle   | Kişi 3 | 5 sa | Orta       |
| HCS audit log yazma               | Kişi 3 | 3 sa | Düşük      |
| MiniKit oylama arayüzü            | Kişi 1 | 4 sa | Düşük      |
| Mirror Node'dan canlı sonuç okuma | Kişi 1 | 2 sa | Düşük      |

> Bu faz tamamlanınca **World ID 4.0 + MiniKit 2.0 track'leri garantilenmiş olur.**

### Faz 3 — Stabilizasyon & Demo Fix (Gün 3, saat 36–48)

| Görev                          | Atanan   | Süre | Risk       |
| ------------------------------ | -------- | ---- | ---------- |
| Uçtan uca akışı test et        | Tüm ekip | 4 sa | **Kritik** |
| Bug fix & edge case            | Tüm ekip | 4 sa | **Kritik** |
| Demo video kaydı (fallback)    | Kişi 1   | 2 sa | Zorunlu    |
| README + GitHub repo temizliği | Kişi 2   | 2 sa | Zorunlu    |

### Faz 4 — Sunum (Gün 3, saat 48–54)

| Görev                          | Süre   | Not                                       |
| ------------------------------ | ------ | ----------------------------------------- |
| Slayt hazırlama (maks 8 slayt) | 2 sa   | Problem → Çözüm → Demo → Track uyumu      |
| Demo script yazma              | 1 sa   | Hangi ekran, hangi cümle, hangi tıklama   |
| Prova 1 & 2 — teknik run       | 1.5 sa | Demo baştan sona 2 kez sorunsuz çalışmalı |
| Prova 3 — jüri simülasyonu     | 1.5 sa | Biri jüri rolü oynayıp soru sorar         |

### Tampon & Fallback Planları

| Senaryo                        | Aksiyon                                             |
| ------------------------------ | --------------------------------------------------- |
| World ID 4.0 API çalışmazsa    | 3.x'e fallback yap, track'i yine hedefle            |
| Hedera testnet kesintisi       | Mirror Node mock data ile demo'ya devam             |
| AgentKit entegrasyonu bitmezse | Faz 2 core'u sun, AgentKit'i "roadmap" olarak anlat |
| Canlı demo kırılırsa           | Önceden kaydedilen fallback videoyu çal             |

### Görev Dağılımı Önerisi

| Kişi   | Sorumluluk                             |
| ------ | -------------------------------------- |
| Kişi 1 | Backend — Hono + IDKit + AgentKit      |
| Kişi 2 | Frontend — Next.js + MiniKit UI        |
| Kişi 3 | Hedera — SDK + HTS + HCS + Mirror Node |

> Bu üç hat büyük ölçüde bağımsız gelişebilir. Entegrasyon Faz 2'nin sonunda yapılır.

### Senaryo Analizi

| Senaryo                   | İhtimal | Sonuç             |
| ------------------------- | ------- | ----------------- |
| Her şey çalışır (5 track) | %20     | $15,000–25,000    |
| 2-3 track teslim edilir   | %55     | $7,000–12,000     |
| Demo kırılır              | %25     | Tüm puanlar düşer |
