# CareerHub — Platforma za poslove i prakse

**Live:**  https://career-hub-1057.onrender.com

> Hostovano na Render free tier-u — prvi load ume da traje ~30 sekundi dok se instanca „probudi".

---

CareerHub povezuje **studente** i **kompanije**. Kompanije postavljaju oglase za poslove i prakse, studenti pretražuju ponude, prijavljuju se i čuvaju oglase, prate statistiku zarada i čitaju IT vesti. Administrator upravlja korisnicima i odobrava kompanije.

## Tehnologije

- **Next.js 16** (App Router, Route Handlers, Turbopack)
- **PostgreSQL** + **Prisma ORM**
- **Tailwind CSS** (brutalist UI)
- **Recharts** (grafovi)
- **JWT** autentifikacija (httpOnly cookie) + **bcrypt**
- **Adzuna API** (statistika plata), **GNews API** (IT vesti)
- **Swagger** (API dokumentacija) · **Vitest** (testovi)

## Funkcionalnosti po ulogama

**Student**
- Registracija i prijava
- Pretraga i pregled oglasa za posao i praksu
- Prijava na oglas (uz proveru roka i statusa oglasa)
- Čuvanje/uklanjanje oglasa („Sačuvani oglasi")
- Pregled i povlačenje sopstvenih prijava
- Uređivanje profila (indeks, opis, CV, avatar)

**Kompanija**
- Registracija (čeka odobrenje administratora)
- Kreiranje, izmena i brisanje oglasa
- Pregled prijava po oglasu i promena statusa (prihvaćeno / odbijeno)
- Uređivanje profila kompanije

**Administrator**
- Pregled statistike (korisnici, kompanije, oglasi, prijave)
- Upravljanje korisnicima (ban/unban — banovan korisnik ne može da se prijavi)
- Odobravanje / odbijanje kompanija

**Javno / integracije**
- Statistika plata (Adzuna) i grafovi
- IT vesti (GNews)
- API dokumentacija na `/docs` (Swagger)

---

## Pokretanje lokalno

Potrebno: **Node.js 20+** i **PostgreSQL**.

### 1. Kloniranje

```bash
git clone https://github.com/andjelkovicmladen/internet-tehnologije-2025-vebaplikacijazaposloveprakse_2022_1057.git
cd internet-tehnologije-2025-vebaplikacijazaposloveprakse_2022_1057
```

### 2. Instalacija zavisnosti

```bash
npm install
```

### 3. `.env` fajl

Napravi `.env` u korenu projekta:

```env
DATABASE_URL="postgresql://postgres:VASA_SIFRA@localhost:5432/poslovi_prakse?schema=public"
JWT_SECRET="vasa_tajna_rec"
ADZUNA_APP_ID="vas_id"
ADZUNA_APP_KEY="vas_kljuc"
GNEWS_API_KEY="vas_gnews_kljuc"
```

> `ADZUNA_*` i `GNEWS_API_KEY` su opcioni — bez njih aplikacija radi, samo stranice *Plate* i *Vesti* neće prikazivati podatke.
> Ključevi su besplatni: [Adzuna](https://developer.adzuna.com/) · [GNews](https://gnews.io/).

### 4. Inicijalizacija baze

```bash
npx prisma generate
npx prisma db push
npx prisma db seed
```

### 5. Pokretanje

```bash
npm run dev
```

Aplikacija: http://localhost:3000 · Prisma Studio: `npx prisma studio`

---

## Test nalozi (nakon `db seed`)

| Uloga | Email | Lozinka |
|-------|-------|---------|
| Admin | `admin@careerhub.com` | `admin123` |
| Kompanija | `microsoft@careerhub.com` (i `google@`, `infostud@`) | `company123` |
| Student | `marko@careerhub.com` (i `ana@`, `petar@`) | `student123` |

---

## Skripte

```bash
npm run dev      # razvojni server
npm run build    # produkcioni build (prisma generate + next build)
npm run start    # pokretanje produkcionog build-a
npm run lint     # ESLint
npm test         # Vitest (unit testovi poslovne logike)
```

---

## Struktura projekta (skraćeno)

```
prisma/          Prisma šema, migracije i seed
src/
  app/
    (public)/    javne i korisničke stranice (App Router)
    api/         Route Handlers (auth, ads, application, companies, admin, news, salary, stats)
  components/    UI komponente (dashboards, profili, kartice, modali)
  services/      klijentski servisi (fetch pozivi ka API-ju)
  lib/           db klijent, validatori, helperi, testovi logike
  types/         deljeni TypeScript tipovi
  proxy.ts       middleware (JWT provera, prosleđivanje x-user-* headera)
```

---

## Docker (opciono)

```bash
docker compose up --build
```

Podiže aplikaciju i PostgreSQL bazu (vidi `compose.yaml`).
