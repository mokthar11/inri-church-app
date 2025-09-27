# INRI Church App - Product Requirements Document

## 1. Projektöversikt

### 1.1 Bakgrund
INRI Church är en fristående församling i centrala Stockholm som brinner för att ge människor hoppet, kärleken och upprättelsen som finns hos Jesus Kristus. Församlingen behöver en dedikerad mobilapp för att stärka gemenskapen och förbättra kommunikationen mellan medlemmar.

### 1.2 Projektmål
Utveckla en mobil församlingsapp som:
- Förbättrar kommunikation mellan församlingsmedlemmar
- Stärker gemenskapen genom digitala verktyg
- Underlättar delning av information och resurser
- Stödjer missionsarbetet genom synlighet och engagemang

### 1.3 Målgrupp
- Primär: Aktiva medlemmar i INRI Church
- Sekundär: Potentiella medlemmar och besökare
- Åldersgrupp: Alla åldrar (design för tillgänglighet)

## 2. Funktionella Krav

### 2.1 Autentisering & Användarhantering
**Krav ID:** FR-001
- Säker inloggning för medlemmar
- Användarroller: Medlem, Pastor, Admin
- Profil med kontaktuppgifter och foto
- Möjlighet att återställa lösenord

### 2.2 Hem (Nyhetsflöde)
**Krav ID:** FR-002
- **Primär funktion:** Första sida användare ser vid inloggning
- **Innehåll:**
  - Nyheter och meddelanden från Pastorer och Admins
  - Kommande evenemang och aktiviteter
  - Andaktstankar och bibelverser
  - Viktiga datum och påminnelser
- **Funktioner:**
  - Chronologisk vy av inlägg
  - Like/reaktioner på inlägg
  - Kommentering av inlägg
  - Push-notifikationer för nya inlägg
  - Möjlighet att dela inlägg

### 2.3 Meddelanden
**Krav ID:** FR-003
- **Personliga meddelanden:**
  - En-till-en chattar mellan medlemmar
  - Meddelandehistorik
  - Läskvitton
- **Gruppmeddelanden:**
  - Dedikerade chattar för arbetsgrupper
  - Möjlighet att skapa nya grupper
  - Administratörsrättigheter för gruppledare
  - Fildelning i grupper
- **Generella funktioner:**
  - Push-notifikationer för nya meddelanden
  - Emoji-reaktioner
  - Möjlighet att citera/svara på specifika meddelanden

### 2.4 Mission
**Krav ID:** FR-004
- **Innehåll:**
  - Information om pågående missionsprojekt
  - Uppdateringar från missionsresor
  - Böneämnen relaterade till missionsarbete
  - Donationsmöjligheter
- **Funktioner:**
  - Foto- och videogallerier från missionsresor
  - Interaktiv karta över missionsområden
  - Möjlighet att "adoptera" specifika missionsprojekt
  - Delning av missionsberättelser

### 2.5 Bibliotek
**Krav ID:** FR-005
- **Videobibliotek:**
  - Kategoriserade predikningar och undervisningar
  - Sökfunktion efter ämne, datum, talare
  - Offline-nedladdning för senare visning
  - Playlistfunktioner
- **Säkerhet:**
  - Endast tillgängligt för inloggade medlemmar
  - Kopieringsskydd/vattenmärkning
- **Funktioner:**
  - Bokmärken och favoriter
  - Delning av videolänkar internt
  - Kommentarer och diskussioner per video

## 3. Icke-funktionella Krav

### 3.1 Design & Användarupplevelse
- **Färgschema:** Baserat på INRI Church webbplats (klassiska kristna färger)
  - Primärfärger: Djupblå (#1B365D), Guld (#D4AF37)
  - Sekundärfärger: Vit (#FFFFFF), Ljusgrå (#F5F5F5)
  - Accentfärger: Mörkgrå (#333333), Röd (#8B0000)
- **Typografi:** Läsbar och professionell
- **Ikoner:** Kristna symboler och moderna UI-ikoner
- **Responsiv design:** Fungerar på alla skärmstorlekar

### 3.2 Prestanda
- Laddningstid max 3 sekunder
- Stöd för offline-funktionalitet
- Optimerad videostreamning
- Effektiv batterianvändning

### 3.3 Säkerhet
- End-to-end kryptering för meddelanden
- GDPR-compliance
- Säker filuppladdning och -nedladdning
- Regelbundna säkerhetsuppdateringar

### 3.4 Plattformar
- iOS (version 14+)
- Android (API level 21+)
- Eventuell webb-version för desktop

## 4. Teknisk Arkitektur & Tech Stack

### 4.1 Supabase-baserad Tech Stack

#### **Frontend (Mobilapp)**
- **React Native** + **Expo**
  - Fördelar: Cross-platform utveckling, snabb utveckling, stor community
  - **Supabase JavaScript Client** för databas och auth integration

#### **Backend (Supabase Ecosystem)**
- **Supabase** (All-in-one Backend-as-a-Service)
  - **PostgreSQL databas** (inbyggd i Supabase)
  - **Supabase Auth** för användarhantering
  - **Supabase Storage** för filer och videos
  - **Supabase Realtime** för live-meddelanden
  - **Row Level Security (RLS)** för datasäkerhet
  - **Supabase Edge Functions** för custom backend-logik

#### **Storage & CDN**
- **Supabase Storage** för videofiler och bilder
  - Inbyggd CDN för snabb leverans
  - Automatisk bildoptimering
  - Säker filuppladdning med RLS

#### **Realtid & Notifikationer**
- **Supabase Realtime** för live-messaging och uppdateringar
- **Expo Push Notifications** för mobilnotifikationer
- **Supabase Database Webhooks** för att trigga notifikationer

#### **Videomanagement**
- **Supabase Storage** för videohosting
- **Expo AV** för videouppspelning i React Native
- **Video.js** som backup för webb-versioner

#### **Autentisering & Säkerhet**
- **Supabase Auth** med social logins och magic links
- **Row Level Security (RLS)** för finkornig åtkomstkontroll
- **JWT tokens** (hanteras automatiskt av Supabase)

### 4.2 Utvecklingsmiljö
- **Version Control:** Git med GitHub/GitLab
- **CI/CD:** GitHub Actions eller GitLab CI
- **Projekthantering:** Jira eller Linear
- **Design:** Figma för UI/UX design

### 4.3 Supabase Implementation Plan

#### **Databasschema (PostgreSQL)**
```sql
-- Användare (utökar Supabase auth.users)
CREATE TABLE profiles (
  id UUID REFERENCES auth.users PRIMARY KEY,
  email TEXT,
  full_name TEXT,
  avatar_url TEXT,
  role TEXT CHECK (role IN ('member', 'pastor', 'admin')),
  phone TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Nyhetsflöde
CREATE TABLE posts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  author_id UUID REFERENCES profiles(id),
  title TEXT NOT NULL,
  content TEXT NOT NULL,
  image_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Kommentarer på inlägg
CREATE TABLE post_comments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  post_id UUID REFERENCES posts(id) ON DELETE CASCADE,
  author_id UUID REFERENCES profiles(id),
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Meddelanden
CREATE TABLE messages (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  conversation_id UUID NOT NULL,
  sender_id UUID REFERENCES profiles(id),
  content TEXT NOT NULL,
  message_type TEXT DEFAULT 'text',
  file_url TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Konversationer (både 1-till-1 och grupper)
CREATE TABLE conversations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT, -- null för 1-till-1, namn för grupper
  is_group BOOLEAN DEFAULT false,
  created_by UUID REFERENCES profiles(id),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Deltagare i konversationer
CREATE TABLE conversation_participants (
  conversation_id UUID REFERENCES conversations(id) ON DELETE CASCADE,
  user_id UUID REFERENCES profiles(id),
  joined_at TIMESTAMP DEFAULT NOW(),
  PRIMARY KEY (conversation_id, user_id)
);

-- Mission information
CREATE TABLE missions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  description TEXT,
  location TEXT,
  status TEXT CHECK (status IN ('planning', 'active', 'completed')),
  start_date DATE,
  end_date DATE,
  image_urls TEXT[],
  created_at TIMESTAMP DEFAULT NOW()
);

-- Videobibliotek
CREATE TABLE videos (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  description TEXT,
  video_url TEXT NOT NULL,
  thumbnail_url TEXT,
  speaker TEXT,
  category TEXT,
  duration INTEGER, -- i sekunder
  upload_date TIMESTAMP DEFAULT NOW(),
  is_featured BOOLEAN DEFAULT false
);
```

#### **Row Level Security (RLS) Policies**
```sql
-- Endast medlemmar kan se innehåll
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE posts ENABLE ROW LEVEL SECURITY;
ALTER TABLE messages ENABLE ROW LEVEL SECURITY;
ALTER TABLE videos ENABLE ROW LEVEL SECURITY;

-- Exempel på RLS policy för posts
CREATE POLICY "Endast medlemmar kan se inlägg" ON posts
  FOR SELECT USING (
    EXISTS (
      SELECT 1 FROM profiles 
      WHERE profiles.id = auth.uid()
    )
  );

-- Endast pastorer och admins kan skapa inlägg
CREATE POLICY "Pastorer kan skapa inlägg" ON posts
  FOR INSERT WITH CHECK (
    EXISTS (
      SELECT 1 FROM profiles 
      WHERE profiles.id = auth.uid() 
      AND role IN ('pastor', 'admin')
    )
  );
```

#### **Supabase Edge Functions**
- **send-notification**: Skicka push-notifikationer
- **process-video-upload**: Hantera videoprocessering
- **moderate-content**: Innehållsmoderering

#### **Fördelar med Supabase för detta projekt:**
1. **Snabb utveckling**: Färdig auth, databas och API
2. **Realtime**: Inbyggd för meddelanden
3. **Säkerhet**: RLS för finkornig åtkomstkontroll
4. **Skalbarhet**: Automatisk skalning
5. **Kostnad**: Gratis tier, sedan pay-as-you-scale
6. **Dashboard**: Inbyggd admin-panel

## 5. Projektfaser

### Fas 1: MVP (3-4 månader)
- Grundläggande autentisering
- Hem-flöde med enkla inlägg
- Grundläggande meddelanden (1-till-1)
- Enkel videolista i biblioteket

### Fas 2: Utökad funktionalitet (2-3 månader)
- Gruppmeddelanden
- Utökad missionssida
- Avancerad videouppspelning
- Push-notifikationer

### Fas 3: Avancerade funktioner (2-3 månader)
- Offline-funktionalitet
- Avancerad sökning
- Detaljerad missionsinfo
- Admin-panel för innehållshantering

## 6. Risker & Utmaningar

### 6.1 Tekniska Risker
- Videostreamning och bandbredd
- Realtid-messaging prestanda
- Cross-platform kompatibilitet

### 6.2 Användbarhet
- Adoption bland äldre medlemmar
- Behov av utbildning och support

### 6.3 Underhåll
- Kontinuerlig uppdatering av innehåll
- Moderering av gruppmeddelanden
- Teknisk support

## 7. Budget & Resurser

### 7.1 Utvecklingsteam (rekommendation)
- 1 Projektledare/Produktägare
- 1 UI/UX Designer
- 2 Fullstack-utvecklare (React Native + Backend)
- 1 DevOps/Infrastructure specialist

### 7.2 Månatliga driftskostnader med Supabase (uppskattning)
- **Supabase Pro**: $25/månad (för production)
- **Storage & Bandwidth**: $0.021/GB storage + $0.09/GB bandwidth
- **För 1000 aktiva användare**: ~300-800 SEK/månad totalt
- **App Store avgifter**: 1000 SEK/år
- **Expo Push Notifications**: Gratis upp till 1M/månad

### 7.3 Utvecklingskostnad med Supabase (reviderad uppskattning)
- **MVP**: 200,000 - 350,000 SEK (lägre pga Supabase)
- **Komplett app**: 400,000 - 700,000 SEK

## 8. Framtida Utveckling

### 8.1 Potentiella Utökningar
- Live-streaming av gudstjänster
- Kalendeintegration för evenemang
- Donationssystem
- Bönelistor och bönegrupper
- QR-kod check-in för gudstjänster
- Integration med församlingens hemsida

### 8.2 Internationell Expansion
- Flerspråksstöd
- Timezone-hantering för globala missioner
- Lokala betalningslösningar

## 9. Success Metrics

### 9.1 Användarengagemang
- Dagliga aktiva användare (DAU)
- Tid spenderad i appen
- Antal meddelanden skickade per dag
- Videovisningar och engagemang

### 9.2 Tekniska Metrics
- App-laddningstider
- Crash rate < 1%
- Push-notifikation öppningsgrad > 20%

### 9.3 Affärsmål
- 80% av medlemmar laddar ner appen inom 6 månader
- 60% använder appen veckovis
- Förbättrad intern kommunikation (mäts genom enkäter)

---

**Dokumentversion:** 1.0  
**Skapad:** September 2025  
**Senast uppdaterad:** September 2025  
**Nästa granskning:** Oktober 2025