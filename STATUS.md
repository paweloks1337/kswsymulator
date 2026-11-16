# Status projektu (stan faktyczny)

## Zweryfikowane automatycznie
- **Silnik** (`packages/engine`): ~90 testów jednostkowych, symulacje balansu (`test:balance`), kontrola typów (strict).
- **Service worker**: 6 testów w Node (`npm -w apps/web run test:pwa`).
- **Test dymny w prawdziwej przeglądarce (Chromium)**: rejestracja, kreator, trening, media, kontrakt, przewijanie czasu, pełna walka rundami z przerwami i wynikiem, wszystkie 14 ekranów, wylogowanie i logowanie, układ mobilny (brak poziomego przewijania). Wykonany na prawdziwym kodzie `apps/web` i `apps/api/src/server.ts` z atrapami: Fastify, Prisma (baza w pamięci), zustand, react-router i odtwarzacza 3D.
- Kontrola typów całego frontendu i API (z uproszczonymi deklaracjami bibliotek zewnętrznych).

## Niezweryfikowane (brak sieci w środowisku, w którym powstał kod)
- Prawdziwe zależności: `npm install`, Fastify, Prisma, migracje PostgreSQL, Vite, Tailwind 3 (zweryfikowano ekwiwalent w Tailwind 4).
- **Grafika 3D** (`apps/web/src/fight3d`): choreografia policzona liczbowo, ale nigdy nie wyrenderowana. Wygląd i płynność wymagają oceny na urządzeniu.
- Service worker w realnej przeglądarce, instalacja PWA na Androidzie i iPhonie.
- Typy `@react-three/fiber` mogą wymagać drobnych poprawek (`npm run typecheck`).

## Brakuje względem pierwotnych założeń
- Model 3D zawodnika na dashboardzie.
- Fizyka Rapier, osobne animacje klinczu i bloków, modele GLB (postacie są proceduralne).
- Walki o tytuł dla gracza, zmiana klubu, negocjacje kontraktów (oferty można tylko przyjąć lub zignorować).
- Sen, nawodnienie i fizjoterapia jako osobne mechaniki zdrowia.
- Testy API i interfejsu w repozytorium (poza `e2e/smoke.mjs`).
- Dane i wygląd NPC są generowane losowo (fikcyjne federacje, zgodnie z założeniem).
