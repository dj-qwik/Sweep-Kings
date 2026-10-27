# Sweep Kings

A party knockout game for up to four players, in the style of a Jump Showdown final. Everyone's a little
royal on a round platform over a pit. A low bar (a giant broom) sweeps round and gets faster and faster:
jump it. A slow high bar at chest height knocks over anyone standing: duck or dive under it. From 30 seconds in, a wedge of the platform
flashes and drops out every 13 seconds until only two are left. Shots freeze people for half a second.
Last one standing wins.

- `server.js` serves the game at `/` and relays online rooms at `/ws`. It has no dependencies.
- `public/index.html` is the whole game.
- `public/music/` holds the soundtrack, Sweep Kings 1 to 6. Track 1 loops in the menus; the others rotate through matches.
- `render.yaml` tells Render how to run it on the free plan.

## Put it online (one time)

1. In the Render dashboard choose **New → Blueprint**, connect your GitHub account if asked, pick the
   `Sweep-Kings` repo, and choose **Apply**. Render reads `render.yaml` and creates a free web service
   called `sweep-kings`.
2. When the deploy finishes, Render shows a link like `https://sweep-kings-xxxx.onrender.com`. That's the
   game. Send it to friends.

Every push to `main` redeploys automatically in a minute or two. Anyone in a match at that moment gets
disconnected.

## Playing online

1. Open the link, pick **Online**, and choose **Create a room**. You'll get a four-letter code.
2. Friends open the same link, pick **Online**, type the code and press **Join**.
3. The host presses **Start**.

Nobody needs a Claude or Render account to play.

## Good to know

- **First load can take about a minute.** On the free plan Render puts the server to sleep after 15
  minutes with nobody connected. The first visit wakes it up.
- **Limits.** Up to 8 connections per room (4 players plus spectators). Render's free plan includes 750
  hours a month per workspace, shared across all your services.

## Testing without a browser

- `node tools/sim.js public/index.html` runs the whole game headless (a stand-in for three.js and the
  page), joins a keyboard player, plays about 40 seconds and reports any runtime errors.
- `node tools/bench.js public/index.html [rounds] [skills] [noguns] [proxy] [log]` plays all-CPU rounds
  per skill and reports how long CPUs last and what got them (low bar, high bar, shot, floor). `proxy`
  makes P1 a stand-in human (a Hard brain that never shoots) that the CPUs hunt; `log` prints details of
  each death.
