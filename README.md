# 🍭 Sweet Town Race — iOS submission guide

A gentle racing game for kids. iPhone-only, built with Capacitor and Codemagic — **no Mac needed**.

- **App name:** Sweet Town Race
- **Bundle ID:** `com.elanstech.sweettownrace`
- **Team ID:** GL35US35RK
- **Apple ID:** elanba.amine@gmail.com

---

## Step 1 — Get the files onto GitHub (Termux)

```bash
termux-setup-storage          # only needed once, then allow the prompt
pkg install git unzip -y

cd ~/storage/downloads
unzip -o sweet-town-race.zip -d ~/sweettownrace
cd ~/sweettownrace
```

Create an **empty** repo on github.com named `sweet-town-race` (no README, no .gitignore),
then:

```bash
git init
git add .
git commit -m "Sweet Town Race v1.0"
git branch -M main
git remote add origin https://github.com/ELANS-TECH/sweet-town-race.git
git push -u origin main
```

Use a **personal access token** as the password if git asks.

---

## Step 1b — About the app icon (already done)

`assets/icon.png` is your Canva icon, already prepared for the App Store:

- exactly **1024 × 1024**, RGB, **no alpha channel** (Apple rejects icons with transparency)
- the baked-in rounded corners were **filled back in** so the artwork is full-bleed square —
  iOS applies its own corner mask, and a pre-rounded icon shows pale slivers at the edges
- `assets/splash.png` and `assets/splash-dark.png` were rebuilt from the same artwork

Codemagic runs `capacitor-assets generate` on every build, so every icon size in the app comes
from this one file. To change the icon later, just replace `assets/icon.png` (1024 × 1024, no
transparency) and push.

---

## Step 2 — Turn on GitHub Pages (privacy + support URLs)

Repo → **Settings → Pages** → Source: `Deploy from a branch` → Branch: `main`, Folder: `/docs` → Save.

After a minute you'll have:

- Support: `https://elans-tech.github.io/sweet-town-race/`
- Privacy: `https://elans-tech.github.io/sweet-town-race/privacy.html`

App Store Connect needs both. Open them once to confirm they load.

---

## Step 3 — Register the App ID

developer.apple.com → **Certificates, IDs & Profiles → Identifiers → +**

- Type: **App IDs → App**
- Description: `Sweet Town Race`
- Bundle ID: **Explicit** → `com.elanstech.sweettownrace`
- Capabilities: leave everything **off** (the game needs none)

---

## Step 4 — Create the app record

App Store Connect → **Apps → + → New App**

- Platform: **iOS**
- Name: `Sweet Town Race`
- Primary language: English (U.S.)
- Bundle ID: `com.elanstech.sweettownrace`
- SKU: `sweettownrace2026`
- Full access

---

## Step 5 — Build on Codemagic

1. **Add application** → GitHub → pick `sweet-town-race`.
   *If the repo dropdown is empty, use the "connect via URL" option instead — that path works.*
2. Codemagic detects `codemagic.yaml` automatically.
3. Confirm **Teams/Integrations** has the App Store Connect key named exactly **`Codemagic Key`**
   (the workflow refers to it by that name).
4. **Start new build** → workflow `ios-release`.

The build creates the iOS project from scratch, generates the icon and launch screen, forces
iPhone-only, writes the Info.plist permission text, signs, and uploads to **TestFlight**.

### What the workflow handles for you
| Task | Where |
|---|---|
| iOS project creation | `npx cap add ios` on the Mac runner |
| App icon + splash | `capacitor-assets generate` from `assets/` |
| iPhone-only (no iPad screenshots needed) | `TARGETED_DEVICE_FAMILY = 1` |
| Camera / photo permission text | PlistBuddy in "Configure Info.plist" |
| Portrait lock, no status bar | same step |
| Export compliance auto-answer | `ITSAppUsesNonExemptEncryption = false` |
| Build number | Codemagic `$BUILD_NUMBER` |

---

## Step 6 — Test on TestFlight

Install from TestFlight and check:

- [ ] Countdown, steering, ramps, splashes
- [ ] Treat rain — big treats are tappable
- [ ] 📸 photo: camera prompt appears, crop works, face animates while driving
- [ ] Close the app fully and reopen — basket, stars, garage items and photo are still there
- [ ] Sound toggle works, voice only on the home screen

*Note: replacing a photo re-opens the picker rather than the crop screen (only the cropped face
is stored on disk, not the original) — this is intentional and keeps storage small.*

---

## Step 7 — Screenshots (1320 × 2868)

Apple's 2026 spec: the **6.9-inch** size `1320 × 2868` is the one to upload — Apple scales it
down for every smaller iPhone, so this is the only set you need.

Good shots to capture (5 is plenty):
1. Home screen with the car and logo
2. Driving with the store ahead and candies on the road
3. Mid-jump off a ramp with the rainbow trail
4. Arriving at a store with confetti
5. The treat rain, or the 🔧 Garage

If you capture on your Android device, send the images over and they can be converted to exact
1320 × 2868 without stretching.

---

## Step 8 — Fill in the listing

**Category:** Games → primary `Casual`, secondary `Racing`
**Age rating:** 4+ (answer "None" to every content question)

**Kids Category:** leave **off**. The app is fine for kids either way, but that category adds
parental-gate rules with no benefit here.

**App Privacy:** choose **"Data Not Collected"**. This is accurate — the photo never leaves the
device, and there is no analytics or tracking.

**Privacy Policy URL:** `https://elans-tech.github.io/sweet-town-race/privacy.html`
**Support URL:** `https://elans-tech.github.io/sweet-town-race/`

### Suggested text

**Subtitle (30 char max):**
`Drive to the treat stores!`

**Promotional text:**
`17 stores to visit, a garage to customise your car, and no way to ever lose. Made for little drivers.`

**Description:**
```
Sweet Town Race is a gentle driving game made for young children — with no way to lose,
no timers, and no pressure. Just pick a car, pick a store, and drive!

DRIVE TO 17 YUMMY STORES
Chocolate, candy, ice cream, cupcakes, donuts, cookies, cake, popcorn, pancakes, gummy bears,
fries, pizza, juice, waffles — and even the carrot, potato and broccoli farms. Every store is a
giant, unmistakable building you drive right up to.

NOBODY EVER LOSES
Cones and puddles only make you wobble for a moment. There are no crashes, no game-overs and no
timers. Three speeds — Easy, Medium and Hard — so it fits any age, and Easy really is slow.

SIMPLE FOR SMALL HANDS
Hold the left or right side of the screen to steer. That's the whole control scheme. The car
drives itself at a steady speed.

BE THE DRIVER
Take an optional photo and your child appears at the wheel — leaning into turns, stretching on
jumps and dancing at the finish line. Add a passenger for a sibling or parent. Photos stay on
the device and are never uploaded or shared.

COLLECT AND CUSTOMISE
Catch treats raining down at every store, then spend them in the Garage on crowns, helmets,
sunglasses, balloons, racing stripes and stickers. Different items need different treats — so
even the broccoli is worth collecting!

EVERY RACE IS DIFFERENT
Roads are freshly built each time you play, so no two drives are the same.

KEEPSAKES
Every finished race saves a souvenir postcard to the scrapbook.

NO ADS. NO IN-APP PURCHASES. NO ACCOUNTS. NO DATA COLLECTED.
```

**Keywords:**
`kids,toddler,car,racing,driving,game,children,preschool,candy,cars,fun,learning,colors,ages 3 5`

---

## Step 9 — Review notes

Paste this into **App Review Information → Notes**:

```
This is a children's driving game with no accounts, no ads, no in-app purchases and no
network connectivity.

CAMERA: The app has an optional feature that lets a player put their photo in the car so they
appear to be the driver. The photo is cropped and stored ONLY on the device (local storage) and
is NEVER uploaded, transmitted or shared — the app has no server and makes no network requests.
The feature is entirely optional; if camera/photo permission is denied, the game plays normally
with a cartoon driver. The photo can be deleted in-app with the "Remove" button.

No login is required. All content is available immediately.
```

---

## Step 10 — Submit

Attach the TestFlight build → **Add for Review** → **Submit**.

---

## Updating later

```bash
cd ~/sweettownrace
# replace www/index.html with the new game file
git add -A && git commit -m "v1.1" && git push
```

Bump `APP_VERSION` in `codemagic.yaml` for a new App Store version, then start a build.
The build number increments automatically.
