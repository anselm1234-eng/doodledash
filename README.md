<div align="center">

```
██████╗  ██████╗  ██████╗ ██████╗ ██╗     ███████╗██████╗  █████╗ ███████╗██╗  ██╗
██╔══██╗██╔═══██╗██╔═══██╗██╔══██╗██║     ██╔════╝██╔══██╗██╔══██╗██╔════╝██║  ██║
██║  ██║██║   ██║██║   ██║██║  ██║██║     █████╗  ██║  ██║███████║███████╗███████║
██║  ██║██║   ██║██║   ██║██║  ██║██║     ██╔══╝  ██║  ██║██╔══██║╚════██║██╔══██║
██████╔╝╚██████╔╝╚██████╔╝██████╔╝███████╗███████╗██████╔╝██║  ██║███████║██║  ██║
╚═════╝  ╚═════╝  ╚═════╝ ╚═════╝ ╚══════╝╚══════╝╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
```

**draw. guess. dash.**

[![License: MIT](https://img.shields.io/badge/license-MIT-white?style=flat-square&labelColor=09090b)](./LICENSE)
[![Next.js](https://img.shields.io/badge/Next.js-15-white?style=flat-square&labelColor=09090b&logo=next.js)](https://nextjs.org)
[![Firebase](https://img.shields.io/badge/Realtime%20Database-RTDB-white?style=flat-square&labelColor=09090b&logo=firebase)](https://firebase.google.com)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-white?style=flat-square&labelColor=09090b&logo=typescript)](https://typescriptlang.org)

</div>

---

**DoodleDash** is a fast-paced, web-based multiplayer experience where quick thinking meets terrible art skills. Spin up a private lobby, send the invite code to your friends, and take turns sketching prompts while everyone else races to type the correct answer.

Built for security, the game's dictionary is kept strictly on the backend—meaning your friends can't peek at the source code to cheat.

---

## what makes it awesome

- **Live Canvas Sync:** Watch the drawing unfold stroke-by-stroke instantly, powered by Firebase.
- **Private Lobbies:** Generate secure 6-character room codes for you and your friends.
- **Complete Host Authority:** Tweak the rules before you start—set round limits, drawing timers, player caps, and specific word categories.
- **Dynamic Scoring:** Earn bonus points for guessing early, and get rewarded for drawing identifiable masterpieces.
- **Gradual Clues:** Stuck? The game automatically exposes letters as time runs out.
- **Engaging Extras:** Throw emojis, skip bad prompts, and trash-talk in real-time chat.
- **Locked-Down Dictionary:** Zero words are sent to the browser until they are needed. Anti-cheat by design.

---

## the tech stack

| component | technology |
|---|---|
| frontend | Next.js 15 (App Router) |
| live state | Firebase Realtime Database |
| data vault | Firebase Firestore (Admin access only) |
| backend | Firebase Admin SDK |
| styling | Tailwind CSS v4 |
| language | TypeScript 5 |

---

## local setup guide

### 1. get the code
```bash
git clone https://github.com/anselm1234-eng/doodledash.git
cd doodledash
npm install
2. firebase configuration
Go to the Firebase Console and create a new project.

Activate Realtime Database.

Activate Firestore (Native mode).

Navigate to Project Settings → Service Accounts and click Generate new private key (save this file).

3. secrets & environment
bash
cp .env.example .env.local
Open .env.local and drop in your Firebase web config and Admin SDK keys. Don't forget to include the NEXT_PUBLIC_FIREBASE_DATABASE_URL straight from your Firebase dashboard.

4. secure your rules
Push the security rules to Firebase to ensure no one can read the words directly from the client:

bash
npx firebase login
npx firebase use your-project-id
npx firebase deploy --only database,firestore:rules
5. populate the dictionary
The client bundle holds zero words. To inject your custom list into Firestore:

bash
cp scripts/seed-words.example.ts scripts/seed-words.ts
# Open scripts/seed-words.ts and add your categories and words
npm run seed
(Note: scripts/seed-words.ts is ignored by Git so your custom lists remain private. The repo only tracks the example file.)

6. launch
bash
npm run dev
Open http://localhost:3000 and start a game.

directory layout
text
app/
├── api/game/route.ts       backend engine (scoring, hints, validation)
├── room/[roomId]/page.tsx  active game view
└── page.tsx                main landing & join screen

components/
├── Canvas.tsx              real-time drawing surface
├── Chat.tsx                message feed & guessing box
├── HostControls.tsx        game rule configuration
├── FloatingEmojis.tsx      reactive emoji overlays
└── RoomDoodles.tsx         background art

lib/
├── firebase.ts             browser Firebase connection
├── firebaseAdmin.ts        secure server-side connection
├── gameLogic.ts            turn management & room flow
├── words.ts                word masking logic
├── sounds.ts               audio effects
├── store.ts                local player state
└── types.ts                TypeScript interfaces

scripts/
├── seed-words.example.ts   template for word injection
└── seed-words.ts           actual private dictionary (ignored by Git)
where the data lives
data type	destination	reason
lobbies, chat, drawings	Realtime Database	Instant, low-latency sync across all clients
word dictionary	Firestore	Hidden from the client, protected by strict rules
active secret word	Server memory / Firestore	Kept entirely away from the guessing players
anti-cheat architecture
All words are injected into Firestore via the Admin SDK under the wordBank/{category} path. Firestore rules are configured to reject any read request from the client. The browser only receives a masked version of the word (e.g., _ _ _ _).

If you need to update the word pool, simply modify scripts/seed-words.ts and execute npm run seed again.

contribute
Found a bug or have a feature idea? Let me know!

Report a bug

Give feedback

support
If DoodleDash is a hit with your friend group, consider supporting the project!

https://img.shields.io/badge/sponsor-%E2%9D%A4-white?style=flat-square&labelColor=09090b

license
MIT — free and open source. Do whatever you want with it!

<div align="center">
crafted with ♥ by Anselm Wrenna

</div> ```