# Mchakato wa Utoaji Toleo (Release Process)

*Haki Miliki © 2025 Ralph Gonzalez – https://olamip.org*

Hati hii inaeleza mchakato wa kawaida ninaotumia kuunda, kujaribu, na kuchapisha matoleo ya vipimo vya OLAMIP na zana zinazohusiana. Kwa sasa, matoleo yote yanasimamiwa na mimi binafsi.

---

## 1. Uwekaji Toleo (Versioning)

Ninatumia mfumo wa **Semantic Versioning 2.0.0**: https://semver.org/

- **MAJOR** — ninapofanya mabadiliko makubwa yasiyoendana na matoleo ya nyuma.  
- **MINOR** — ninapoongeza vipengele vipya vinavyooana na matoleo ya nyuma.  
- **PATCH** — ninapofanya marekebisho madogo, kusahihisha maneno, au kuboresha ufafanuzi bila kubadilisha tabia ya mfumo.  

Mfano:  
`1.0.0` → `1.0.1` (patch), `1.1.0` (minor), `2.0.0` (major).

---

## 2. Muundo wa Matawi (Branching Model)

Ninatumia mtiririko rahisi wa Git:

- `main` — tawi thabiti, tayari kwa toleo wakati wote.  
- Matawi ya vipengele au marekebisho (mfano: `spec/delta-formats`, `fix/tag-rules`) — matawi ya muda mfupi kwa mabadiliko.  
- Matawi ya toleo (mfano: `release/v1.1.0`) — huundwa kabla ya kuchapisha toleo.  

---

## 3. Masharti Kabla ya Kutoa Toleo

Kabla ya kuanza mchakato wa toleo, ninahakikisha:

- Mabadiliko yote yaliyopangwa kwa toleo hilo yameunganishwa kwenye `main`.  
- Ukaguzi wote wa kiotomatiki (linting, uthibitishaji wa schema, build scripts) umepita.  
- Masuala yote yanayohusiana na toleo hilo yamefungwa au yameandikwa.  
- `CHANGELOG.md` imesasishwa kwa maelezo ya wazi na yanayosomeka.  
- Nyaraka zote ziko sahihi, ikijumuisha:  
  - `file-format-specification.md`  
  - `olamip-delta-file-format-specification.md`  
  - `docs/introduction.md`  
  - `faq.md`  
- Faili za mfano `olamip.json` na `olamip-delta.json` bado zinathibitishwa dhidi ya sheria za sasa.  

---

## 4. Kuunda Release Candidate

1. Tengeneza tawi la toleo kutoka `main`:

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Sasisha namba za toleo (ikiwa zinatumika kwenye zana, script, au mali nyingine).  
3. Sasisha `CHANGELOG.md` kwa kichwa cha toleo na maelezo ya mabadiliko.  
4. Fanya commit:

   ```bash
   git add .
   git commit -m "chore(release): prepare v1.1.0"
   ```

5. Sukuma tawi:

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Endesha uthibitishaji wa ndani au CI tena na uhakikishe kila kitu kimepita.  

---

## 5. Kupima Release Candidate

Kwa kuwa mimi ndiye ninayesimamia matoleo yote, majaribio yangu binafsi ndiyo kipimo kikuu:

- Ninathibitisha kuwa faili za mfano `olamip.json` na `olamip-delta.json` bado zinaendana na schema.  
- Ninahakikisha kuwa mifano yote kwenye nyaraka za vipimo bado inafuata sheria zilizoelezwa.  
- Ninasoma sehemu zote zilizosasishwa ili kugundua kutokuwepo kwa mwendelezo au makosa.  

Nikigundua matatizo makubwa:

- Nayarekebisha kwenye tawi la `release`.  
- Ninasasisha `CHANGELOG.md` na kufanya commit tena.  
- Ninarudia uthibitishaji kabla ya kuendelea.  

---

## 6. Kukamilisha Toleo

Baada ya Release Candidate kuwa thabiti:

1. Weka tag ya toleo kwenye Git:

   ```bash
   git tag -a vX.Y.Z -m "Release v1.1.0"
   git push origin vX.Y.Z
   ```

2. Tengeneza GitHub Release:

   - Tumia tag `vX.Y.Z`.  
   - Nakili sehemu husika ya `CHANGELOG.md` kwenye maelezo ya toleo.  
   - Ambatanisha mafaili yoyote yanayozalishwa (mfano: schema, zana za CLI, snapshot tarballs) ikiwa yapo.  

---

## 7. Hatua Baada ya Toleo

Baada ya kuchapisha toleo:

- Ninasasisha maelezo ya ndani au ramani ya njia (roadmap).  
- Ninahamisha vipengele vilivyocheleweshwa kwenda kwenye orodha ya baadaye.  
- Ikiwa kuna njia ya mawasiliano (mfano: Twitter, jarida, GitHub Discussions), ninaweza kutangaza toleo jipya.  

---

## 8. Utawala na Usimamizi

Hata kama kuna msimamizi mmoja, ninaweka safu ya utawala:

- Mabadiliko yoyote yanayoathiri schema kuu, sehemu za lazima, au toleo la itifaki lazima yaandikwe wazi kwenye changelog.  
- Mabadiliko makubwa kwenye mchakato wa utoaji toleo lazima yawekwe kwenye rekodi fupi ya maamuzi ndani ya `governance/decision-records/`.  

---

## 9. Kurudisha Toleo au Kutengeneza Patch

Ikiwa toleo lina hitilafu kubwa:

- Ninatengeneza patch haraka iwezekanavyo (mfano: `v1.1.1`).  
- Ninasasisha `CHANGELOG.md` kwa maelezo ya marekebisho.  
- Ikiwa ni lazima, ninaboresha nyaraka au kutangaza kuwa toleo la awali halipendekezwi.  

---

## 10. Uotomatishaji na Upanuzi wa Baadaye

Ninaweka otomatiki kadri inavyowezekana:

- Hatua zinazojirudia kama:  
  - Kuangalia uhalali wa JSON  
  - Kuhakikisha faili za mfano zinaendana na schema  
  - Kutengeneza au kulainisha maandishi ya changelog  

Ninaweka mchakato rahisi ili baadaye, ikiwa wachangiaji wataongezeka, iwe rahisi kushirikiana.

---

Mchakato huu wa utoaji toleo unamilikiwa na kusimamiwa na mimi, na unaweza kusasishwa kupitia mtiririko wa kawaida wa kazi ninapoboresha taratibu zenyewe.
