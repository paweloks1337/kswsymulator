# KSW Career Simulator

Stan faktyczny, co przetestowano i czego brakuje: `STATUS.md`. Raport balansu: `BALANCE.md`.

Monorepo: `packages/engine` (logika gry, czysty TS), `apps/api` (Fastify + Prisma + PostgreSQL), `apps/web` (React + Vite + Tailwind).

    cp .env.example apps/api/.env   # ustaw JWT_SECRET
    docker compose up -d            # PostgreSQL
    npm install
    npm run db:migrate              # tworzy tabele
    npm test                        # testy silnika
    npm -w packages/engine run test:balance   # symulacje balansu (ok. 20 s)
    npm -w apps/web run test:pwa              # testy service workera
    npm run dev:api                 # API na :3001
    npm run dev                     # web na :5173 (proxy /api -> :3001)

Serwer jest źródłem prawdy: trening, czas, kontrakty i walki liczy silnik na serwerze. Po zmianie schematu: npm run db:migrate.
Moduły oznaczone "etap N" w menu to jawne ekrany statusu, nie działające funkcje.

## Walka 3D
`apps/web/src/fight3d`: odtwarzacz walki w React Three Fiber. Silnik zapisuje oś czasu (`FightResult.timeline`), a odtwarzacz tylko ją pokazuje.
Postacie są proceduralne (szkielet z grup, proporcje z wzrostu i budowy), animowane kodem. Własne modele GLB można podpiąć w `Fighter3D.tsx`.

## Sztab
Trenerzy, fizjoterapeuta i menedżer wpływają na tempo rozwoju, zmęczenie, leczenie kontuzji i gaże. Logika: `packages/engine/src/staff.ts`.

## Taktyka w trakcie walki
Walka toczy się rundami: przed walką i po każdej rundzie wybierasz agresję i plan (presja, dystans, obalenia, kontry, oszczędzanie kondycji, korpus). Trener główny podpowiada na podstawie liczb z rundy. Stan walki zapisuje się na serwerze, więc można ją dokończyć po przeładowaniu strony.

## Media i sponsorzy
Wywiady, media społecznościowe, konferencje, trash talk i face-offy zmieniają popularność, reputację i kontrowersje. Popularność podnosi gaże i otwiera sponsorów, skandal może zerwać umowę, a porażka po głośnych zapowiedziach kosztuje podwójnie. Logika: `packages/engine/src/media.ts`.

## Żyjący świat
NPC walczą między sobą, doznają kontuzji, rozwijają się (młodzi) lub tracą formę (starsi), starzeją się co rok, kończą kariery (zastępują ich awansujący zawodnicy lub nowe talenty), awansują i spadają między federacjami oraz co miesiąc walczą o tytuły. Logika: `simulateWorld` w `packages/engine/src/world.ts`.

## Balans
Raport z symulacji botów: `BALANCE.md`. Mnożniki balansu: `GAIN_SCALE` (trening), gaże i progi federacji (`world.ts`), koszty życia (`finance.ts`).

## Styl życia
Mieszkanie (pokój, wynajem, apartament, dom na własność), samochód i domowa siłownia: `packages/engine/src/lifestyle.ts`. Wpływają na regenerację, dojazdowe zmęczenie, koszt i skuteczność siłowni, popularność oraz majątek.

## PWA i telefon
`apps/web/public`: manifest, ikony i `sw.js`. Po `npm run build` aplikację można zainstalować na Androidzie (przycisk w Ustawieniach) i iPhonie (Safari, Udostępnij, Dodaj do ekranu początkowego). Service worker zapisuje powłokę aplikacji i zasoby, więc ekrany otwierają się offline po pierwszej wizycie. Zapytania do `/api` nigdy nie są buforowane, więc dane kariery zawsze pochodzą z serwera.
Układ mobilny: dolna nawigacja, bezpieczne marginesy (notch), większe cele dotyku. Grafika 3D dobiera rozdzielczość do urządzenia i obniża ją, gdy FPS spada poniżej ok. 40. Moduł 3D jest osobną paczką ładowaną dopiero przy oglądaniu walki.
Service worker działa tylko po `npm run build` (w `npm run dev` jest wyłączony). Wymaga HTTPS poza localhostem.

## Kontrola jakości
    npm run typecheck   # typy: silnik, API, web
    npm run test:all    # testy silnika i service workera
    npm run e2e         # test dymny w przeglądarce (wymaga działającego API, web i bazy)
