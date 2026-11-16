# Raport balansu

Metoda: bot (`packages/engine/src/balance.ts`) gra całe kariery w silniku według czterech polityk, na stylach boks, zapasy, MMA i BJJ.
Wyniki z 16 karier na politykę, 3 lata gry (1095 dni). To mała próba, więc liczby są orientacyjne.

| Polityka | Ocena start → rok 1 → rok 2 → rok 3 | Saldo po 3 latach (mediana) | Wygrane | Iron Cage / Apex | Kontuzje na 100 dni treningu |
|---|---|---|---|---|---|
| pełen (trening, sztab, media) | 36 → 44 → 49 → 52 | ok. 208 tys. zł | 68% | 100% / 100% | 5,9 |
| tylko trening | 36 → 43 → 47 → 49 | ok. 64 tys. zł | 67% | 100% / 69% | 8,0 |
| przetrenowany (zmęczenie do 95) | 36 → 39 → 42 → 43 | ok. 48 tys. zł | 68% | 100% / 0% | 14,0 |
| leń (tylko odpoczynek) | 34,5 bez zmian | ok. 4 tys. zł | 45% | 0% / 0% | 0 |

## Co wykryły symulacje i co zmieniono
1. **Gra była zbyt hojna.** Mediana salda sięgała setek tysięcy po 3 latach, a walka przypadała co ok. 30 dni (37 walk w 3 lata). Teraz obóz trwa 56, 70 lub 84 dni (zależnie od federacji), czyli ok. 5 walk rocznie, a gaże i koszty życia zostały przeliczone.
2. **Najwyższe federacje były nieosiągalne.** Wymagana ocena 50 i 62 leżała ponad zasięgiem NPC (mediany 38, 43, 47). Progi to teraz 40 (Iron Cage) i 47 (Apex).
3. **Rozwój był za wolny.** Tempo treningu zwiększone (`GAIN_SCALE` 1,3), tak że przy sensownej grze Iron Cage jest osiągalny po ok. roku.
4. **Media dawały sto punktów popularności** przy codziennych wywiadach. Dodano malejące zyski i zmęczenie medialne.
5. **Sponsorzy** zostali przecenieni o ok. 30%.

## Styl życia (mieszkanie, auto, domowa siłownia)
Dodano wydatki z realnym wpływem: regeneracja zmęczenia zależy od mieszkania, auto usuwa dojazdowe zmęczenie i daje status, a domowa siłownia czyni sesje siłowni darmowymi i skuteczniejszymi. Wartość rzeczy odsprzedaje się ze stratą.
Bot z zakupami („pełen+styl") po 3 latach miał ok. 112 tys. zł gotówki i 140 tys. zł majątku wobec 184 tys. zł u bota bez zakupów. Ocena 51,9 wobec 51,4, a wygrane 69% wobec 67%. Wydatki działają więc jako sink (ok. 40% mniej gotówki), a korzyść jest umiarkowana, nie przełamuje balansu.

## Co działa zgodnie z zamierzeniem
Trening się opłaca, przetrenowanie karze kontuzjami i wolniejszym rozwojem, a brak treningu nie bankrutuje zawodnika, ale też nie rozwija.

## Znane problemy
- **Wydatki dodane, ale gotówka nadal rośnie** przy dobrej grze. Dom (220 tys. zł) jest celem końcowym, którego bot nie osiąga w 3 lata.
- **Bankructwo** (6% w polityce „pełen"): bot zatrudnia sztab zbyt agresywnie. Gra nie ma jeszcze konsekwencji długu poza brakiem treningu.
- **Zawodnik gracza przewyższa NPC** w niższych federacjach (win rate 67%). Nie ma jeszcze walk o tytuł dla gracza.
- Próba jest mała, a bot nie korzysta z taktyki między rundami ani z negocjacji. Rzeczywisty balans zależy od ludzkiej gry.

## Jak powtórzyć
`npm run test:balance` w `packages/engine` (ok. 20 s) sprawdza kierunki zależności. Pełny raport można zbudować funkcją `playCareer` z `POLICIES`.
