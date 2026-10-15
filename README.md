# Phantom Demo — Build 141

A local wallet UI simulation. Balances, trades, funding and chat messages are demo data. Market prices and available historical charts come from external data providers.

## Open the app

- Open `www/phantom-standalone.html` for the single-file version.
- Serve `www/` with a local web server for the installable web app. For example, run `python -m http.server 8080 --directory www`, then open http://localhost:8080.
- The existing `.github/workflows/main.yml` packages the prebuilt `www/` assets into an unsigned iOS app. Signing and sideloading remain separate.

## Build 141 app icon

The supplied Phantom artwork is used for the app icon. The rounded mark is used for browser and home-screen icons; the full-square mark is used for the iOS app icon and the maskable web icon. The iOS workflow copies the 1024px icon into the generated Xcode asset catalog.

The source images are in `assets/brand/`. To regenerate all icon sizes after changing them, run `python build/prepare-icons.py` with Pillow installed, then run the normal app build. The standalone HTML embeds the current icons and manifest during each build.

## Build 140 unobtrusive demo information

Removed the first-launch popup and fixed top banner. Demo information is available through Settings → About This Demo. Trade and transaction results still clearly say Simulated.

## Build 139 demo warning (superseded)

The first launch shows a clear independent-demo notice. A persistent top strip says DEMO MODE · NO REAL FUNDS on all screens. Local send/transaction screens now say Simulated and show no on-chain confirmation, slot, network fee, or signature.

## Build 138 refresh adjustment

The centred pull spinner is 36px (up from 24px). After a qualifying pull is released, the content holds for 180ms before settling. This short pause is applied only when refresh is triggered.

## Build 137 refresh

Pull down on Home to refresh. The Home content follows the finger, a 12-spoke grey spinner appears above the balance, and the content settles back in about 0.6 seconds. The top navigation and bottom search controls stay in place. Swipe-up refresh and the gear beside the balance were removed to match the supplied recording. Reduced-motion mode skips the return animation.

## Build 136 Home cards

Token and stock holdings use flat dark-grey rounded cards with 7px gaps, 70px minimum height, 36px logos and compact typography. The Cash card uses the same grey and corner shape. These styles apply to newly purchased holdings too.

## Included changes

- Pull down on Home to refresh prices; the spinner appears above the balance while the content moves.
- Cash uses a banknote icon. Home token logos are bundled to match the supplied visual reference.
- Token detail → Trade opens Buy, Sell and Swap actions. Buying and selling include a payment/receive asset picker, separate amount-unit control, Review, Confirm, processing, completion and failure screens.
- Swap and Add Cash also go through Review. Confirmation checks balances, quote expiry and repeat submission before updating local holdings.
- Review uses the reference's black layout, large amount, Buy/Payment/Frequency rows and lavender confirmation button. Detail and settings drawers use rounded bottom sheets.
- Historical charts use returned timestamps, price/time inspection for line charts and OHLC inspection for candles. Unsupported or unavailable history shows an empty state with retry. LIVE requests refresh every minute using the available daily OHLC endpoint; it is not a tick-by-tick feed.
- Asset pages have a Live Chat preview. Avatar → Chats opens searchable token and market rooms. Rooms include local messages, long-press reactions, message deletion and a Trade shortcut. Prediction sheets include a chat entry point.
- Modal entry/dismissal, chart changes, message arrival and trade status have transitions, with reduced-motion support.

## Demo limits and visual fidelity

No blockchain transaction, payment-provider checkout or remote chat service is connected. The fee displayed in Review is a fixed demo assumption, not a live provider quote. Frequency stays at one-time purchase; recurring execution is not implemented. Slippage controls preview preferences and do not change the fixed demo quote. Perps chat links to the app's existing Perps list.

Official screenshots informed the Review layout and chart controls. Complete current chat screens and exact animation timings were not available in the references, so those details are approximations. The current pull spinner and return motion were adjusted from the supplied video.

See `INTERACTION_REFERENCES.md` for the research sources and `ICON_SOURCES.md` for icon sources.

## Rebuild after source edits

Requires Node.js and npm:

```sh
npm install
npm run build
```

The build writes `www/app.js`, `www/styles.css`, and `www/phantom-standalone.html`. Commit those files before running the existing IPA workflow. The build preserves the original bundled React runtime in `build/vendor-runtime.prefix.js`; the editable app is in `App.jsx`, `InteractionUI.jsx`, `trade-model.js`, and `interaction.css`.

## Build status

The web bundle and standalone HTML were rebuilt with esbuild. Runtime and device tests were not run for this update.

Earlier notes supplied with the project are retained in `build/LEGACY_BUILD_NOTES.md`.
