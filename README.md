<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Creative Playground XL — Interactive Portfolio</title>
  <style>
    :root {
      --bg: #05050a;
      --motion: 1;
      --trail-enabled: 1;
      --text: #f8f4ff;
      --muted: #aaa4be;
      --glass: rgba(255,255,255,.075);
      --glass-2: rgba(255,255,255,.13);
      --border: rgba(255,255,255,.16);
      --pink: #ff4ecd;
      --violet: #8f5cff;
      --cyan: #35e7ff;
      --lime: #c6ff4e;
      --orange: #ffb14e;
      --red: #ff4e6a;
      --radius: 30px;
      --theme-hue: 0deg;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      min-height: 100vh;
      background: var(--bg);
      color: var(--text);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      overflow-x: hidden;
      cursor: none;
    }

    a, button, input, textarea, .clickable {
      cursor: none;
    }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      z-index: -6;
      background:
        radial-gradient(circle at var(--mx, 50%) var(--my, 30%), rgba(143,92,255,.33), transparent 20rem),
        radial-gradient(circle at 20% 10%, rgba(255,78,205,.22), transparent 22rem),
        radial-gradient(circle at 90% 75%, rgba(53,231,255,.2), transparent 24rem),
        #05050a;
      transition: background .12s linear;
    }

    body::after {
      content: "";
      position: fixed;
      inset: 0;
      z-index: -5;
      pointer-events: none;
      opacity: .34;
      background-image:
        linear-gradient(rgba(255,255,255,.06) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,.06) 1px, transparent 1px);
      background-size: 48px 48px;
      mask-image: radial-gradient(circle at center, black, transparent 80%);
      animation: gridMove 32s linear infinite;
    }

    @keyframes gridMove {
      from { background-position: 0 0; }
      to { background-position: 48px 48px; }
    }

    ::selection {
      background: var(--pink);
      color: white;
    }

    .noise {
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 999;
      opacity: .035;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='256' height='256' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");
    }

    .cursor {
      position: fixed;
      will-change: transform, width, height;
      width: 18px;
      height: 18px;
      left: 0;
      top: 0;
      z-index: 9999;
      border: 2px solid white;
      border-radius: 999px;
      pointer-events: none;
      transform: translate(-50%, -50%);
      mix-blend-mode: difference;
      transition: width .2s ease, height .2s ease, border-color .2s ease, background .2s ease;
    }

    .cursor.big {
      width: 64px;
      height: 64px;
      background: rgba(255,255,255,.2);
    }

    .cursor-dot {
      position: fixed;
      will-change: transform;
      width: 5px;
      height: 5px;
      left: 0;
      top: 0;
      z-index: 10000;
      border-radius: 999px;
      background: white;
      pointer-events: none;
      transform: translate(-50%, -50%);
      mix-blend-mode: difference;
    }

    .trail {
      position: fixed;
      will-change: transform, opacity;
      width: 8px;
      height: 10px;
      z-index: 9998;
      pointer-events: none;
      border-radius: 999px;
      background: var(--cyan);
      box-shadow: 0 0 20px var(--cyan);
      opacity: .65;
      transform: translate(-50%, -50%);
      animation: trailFade .7s ease forwards;
    }

    @keyframes trailFade {
      to { opacity: 0; transform: translate(-50%, -50%) scale(.2); }
    }

    .orb {
      position: fixed;
      width: 20vmax;
      height: 20vmax;
      border-radius: 50%;
      filter: blur(30px);
      opacity: .42;
      pointer-events: none;
      z-index: -4;
      animation: drift 12s ease-in-out infinite alternate;
    }

    .orb.one { background: var(--pink); top: 14%; left: 3%; }
    .orb.two { background: var(--cyan); right: 2%; bottom: 10%; animation-delay: -4s; animation-duration: 15s; }
    .orb.three { background: var(--violet); right: 24%; top: 8%; width: 14vmax; height: 14vmax; animation-delay: -7s; animation-duration: 13s; }
    .orb.four { background: var(--lime); left: 38%; bottom: -9%; width: 13vmax; height: 13vmax; animation-delay: -10s; animation-duration: 18s; }

    @keyframes drift {
      from { transform: translate3d(-3vw, 2vh, 0) scale(.88); }
      to { transform: translate3d(6vw, -5vh, 0) scale(1.12); }
    }

    .progress {
      position: fixed;
      inset: 0 0 auto 0;
      height: 4px;
      z-index: 1000;
      transform-origin: left center;
      transform: scaleX(0);
      background: linear-gradient(90deg, var(--pink), var(--violet), var(--cyan), var(--lime));
      box-shadow: 0 0 20px rgba(53,231,255,.6);
    }

    .container {
      width: min(1180px, calc(100% - 36px));
      margin: 0 auto;
    }

    .nav {
      position: fixed;
      top: 18px;
      left: 50%;
      transform: translateX(-50%);
      width: min(1180px, calc(100% - 36px));
      z-index: 100;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 14px 12px 18px;
      border: 1px solid var(--border);
      border-radius: 999px;
      background: rgba(7,7,14,.58);
      backdrop-filter: blur(20px);
      box-shadow: 0 20px 70px rgba(0,0,0,.35);
      animation: navIn .8s cubic-bezier(.16,1,.3,1) both;
    }

    @keyframes navIn {
      from { opacity: 0; transform: translate(-50%, -24px) scale(.96); }
      to { opacity: 1; transform: translate(-50%, 0) scale(1); }
    }

    .brand {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      font-weight: 900;
      letter-spacing: -.04em;
    }

    .brand span:first-child {
      width: 36px;
      height: 36px;
      display: grid;
      place-items: center;
      border-radius: 50%;
      background: conic-gradient(from 90deg, var(--pink), var(--violet), var(--cyan), var(--lime), var(--pink));
      color: #07070e;
      animation: spin 7s linear infinite;
    }

    @keyframes spin { to { transform: rotate(360deg); } }

    .nav-links {
      display: flex;
      gap: 6px;
      align-items: center;
    }

    .nav-links a,
    .tiny-btn {
      position: relative;
      padding: 10px 13px;
      color: var(--muted);
      border-radius: 999px;
      font-size: .92rem;
      overflow: hidden;
      border: 1px solid transparent;
      background: transparent;
      transition: color .2s ease, background .2s ease, border-color .2s ease;
    }

    .nav-links a:hover,
    .tiny-btn:hover {
      color: white;
      background: rgba(255,255,255,.1);
      border-color: rgba(255,255,255,.13);
    }

    .hero {
      position: relative;
      min-height: 100vh;
      display: grid;
      place-items: center;
      padding: 130px 0 80px;
    }

    .hero-grid {
      display: grid;
      grid-template-columns: 1fr .92fr;
      gap: 42px;
      align-items: center;
    }

    .tag {
      display: inline-flex;
      gap: 9px;
      align-items: center;
      width: fit-content;
      padding: 9px 14px;
      margin-bottom: 18px;
      border-radius: 999px;
      border: 1px solid var(--border);
      background: rgba(255,255,255,.07);
      color: var(--muted);
      backdrop-filter: blur(16px);
      animation: rise .7s ease both .12s;
    }

    .tag i {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: var(--lime);
      box-shadow: 0 0 24px var(--lime);
      animation: blink 1.5s infinite;
    }

    @keyframes blink { 50% { opacity: .25; transform: scale(.75); } }

    .hero h1 {
      max-width: 800px;
      font-size: clamp(3.4rem, 9vw, 8.9rem);
      line-height: .86;
      letter-spacing: -.095em;
      animation: rise .8s ease both .25s;
    }

    .hero h1 .outline {
      display: inline-block;
      color: transparent;
      -webkit-text-stroke: 1px rgba(255,255,255,.74);
      text-shadow: 0 0 34px rgba(255,78,205,.2);
      transition: transform .22s ease, color .22s ease, -webkit-text-stroke .22s ease;
    }

    .hero h1 .outline:hover {
      color: var(--pink);
      -webkit-text-stroke: 1px transparent;
      transform: skewX(-8deg) translateY(-6px);
    }

    .gradient {
      background: linear-gradient(110deg, #fff, var(--cyan), var(--pink), var(--lime));
      background-size: 250% 250%;
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      animation: gradientText 4s ease infinite;
    }

    @keyframes gradientText {
      0%,100% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
    }

    .hero p {
      max-width: 650px;
      margin: 28px 0 34px;
      color: var(--muted);
      font-size: clamp(1rem, 2vw, 1.18rem);
      line-height: 1.8;
      animation: rise .8s ease both .38s;
    }

    .actions {
      display: flex;
      flex-wrap: wrap;
      gap: 14px;
      animation: rise .8s ease both .52s;
    }

    .btn {
      position: relative;
      isolation: isolate;
      min-height: 52px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      padding: 0 22px;
      border-radius: 999px;
      border: 1px solid var(--border);
      background: rgba(255,255,255,.075);
      color: white;
      font-weight: 850;
      overflow: hidden;
      transition: transform .18s ease, border-color .18s ease;
    }

    .btn::before {
      content: "";
      position: absolute;
      inset: -2px;
      z-index: -1;
      background: radial-gradient(circle at var(--bx, 50%) var(--by, 50%), rgba(255,255,255,.38), transparent 8rem);
      opacity: 0;
      transition: opacity .2s ease;
    }

    .btn:hover::before { opacity: 1; }
    .btn.primary {
      background: linear-gradient(135deg, var(--pink), var(--violet), var(--cyan));
      border-color: transparent;
      box-shadow: 0 12px 34px rgba(143,92,255,.34);
    }

    .stage {
      position: relative;
      height: 620px;
      border-radius: 42px;
      border: 1px solid var(--border);
      background:
        linear-gradient(150deg, rgba(255,255,255,.14), rgba(255,255,255,.035)),
        radial-gradient(circle at 50% 0, rgba(255,255,255,.14), transparent 20rem);
      box-shadow: 0 18px 54px rgba(0,0,0,.42);
      overflow: hidden;
      transform-style: preserve-3d;
      perspective: 1000px;
      animation: stageIn 1s cubic-bezier(.16,1,.3,1) both .24s;
    }

    @keyframes stageIn {
      from { opacity: 0; transform: translateY(30px) rotateX(8deg) scale(.96); }
      to { opacity: 1; transform: translateY(0) rotateX(0) scale(1); }
    }

    .blob {
      position: absolute;
      width: 210px;
      height: 210px;
      border-radius: 42% 58% 48% 52%;
      background: linear-gradient(135deg, var(--pink), var(--violet));
      filter: drop-shadow(0 14px 24px rgba(0,0,0,.32));
      animation: morph 5s ease-in-out infinite alternate, blobFloat 6s ease-in-out infinite;
    }

    .blob.one { top: 80px; left: 70px; }
    .blob.two { width: 160px; height: 160px; right: 65px; top: 105px; background: linear-gradient(135deg, var(--cyan), var(--lime)); animation-delay: -1.7s; }
    .blob.three { width: 260px; height: 260px; right: 80px; bottom: 65px; background: linear-gradient(135deg, var(--orange), var(--pink)); animation-delay: -3s; }
    .blob.four { width: 115px; height: 115px; left: 52%; top: 45%; background: linear-gradient(135deg, white, var(--cyan)); animation-delay: -4s; }

    @keyframes morph {
      0% { border-radius: 42% 58% 48% 52%; }
      50% { border-radius: 58% 42% 63% 37%; }
      100% { border-radius: 45% 55% 38% 62%; }
    }

    @keyframes blobFloat { 50% { transform: translateY(-24px) rotate(13deg); } }

    .glass-card {
      position: absolute;
      left: 46px;
      bottom: 46px;
      width: min(390px, calc(100% - 92px));
      padding: 22px;
      border-radius: 26px;
      border: 1px solid rgba(255,255,255,.18);
      background: rgba(5,5,10,.52);
      backdrop-filter: blur(20px);
      box-shadow: 0 12px 34px rgba(0,0,0,.28);
    }

    .glass-card h3 { font-size: 1.35rem; margin-bottom: 8px; }
    .glass-card p { color: var(--muted); line-height: 1.55; margin: 0; font-size: .96rem; }

    .sound-bars {
      position: absolute;
      right: 32px;
      bottom: 32px;
      display: flex;
      align-items: end;
      gap: 6px;
      height: 58px;
      padding: 13px;
      border-radius: 18px;
      background: rgba(255,255,255,.08);
      border: 1px solid rgba(255,255,255,.13);
    }

    .sound-bars span {
      width: 7px;
      border-radius: 999px;
      background: white;
      animation: equalize 1s ease-in-out infinite;
      height: calc(12px + var(--h) * 1px);
    }

    .sound-bars span:nth-child(2) { animation-delay: -.2s; }
    .sound-bars span:nth-child(3) { animation-delay: -.4s; }
    .sound-bars span:nth-child(4) { animation-delay: -.1s; }
    .sound-bars span:nth-child(5) { animation-delay: -.35s; }
    @keyframes equalize { 50% { height: 12px; opacity: .48; } }

    @keyframes rise {
      from { opacity: 0; transform: translateY(24px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .ticker {
      border-block: 1px solid var(--border);
      background: rgba(255,255,255,.045);
      overflow: hidden;
      white-space: nowrap;
      padding: 18px 0;
      color: transparent;
      -webkit-text-stroke: 1px rgba(255,255,255,.52);
      font-size: clamp(2rem, 5vw, 4.3rem);
      font-weight: 950;
      letter-spacing: -.06em;
    }

    .ticker-track {
      display: inline-flex;
      gap: 30px;
      animation: ticker 18s linear infinite;
    }

    .ticker:hover .ticker-track { animation-play-state: paused; }
    @keyframes ticker { from { transform: translateX(0); } to { transform: translateX(-50%); } }

    section {
      padding: 105px 0;
    }

    .section-head {
      display: flex;
      justify-content: space-between;
      align-items: end;
      gap: 28px;
      margin-bottom: 36px;
    }

    .section-head h2 {
      font-size: clamp(2.4rem, 6vw, 5.6rem);
      line-height: .9;
      letter-spacing: -.08em;
    }

    .section-head p {
      max-width: 540px;
      color: var(--muted);
      line-height: 1.7;
    }

    .playground {
      display: grid;
      grid-template-columns: repeat(12, 1fr);
      gap: 18px;
    }

    .interactive-card {
      position: relative;
      min-height: 290px;
      padding: 24px;
      border-radius: var(--radius);
      border: 1px solid var(--border);
      background:
        radial-gradient(circle at var(--cx, 50%) var(--cy, 50%), rgba(255,255,255,.18), transparent 11rem),
        rgba(255,255,255,.07);
      overflow: hidden;
      transform-style: preserve-3d;
      transition: transform .18s ease, border-color .18s ease, background .18s ease;
    }

    .interactive-card:hover { border-color: rgba(255,255,255,.38); }
    .interactive-card h3 { position: relative; z-index: 2; font-size: 1.55rem; margin-bottom: 9px; transform: translateZ(50px); }
    .interactive-card p { position: relative; z-index: 2; color: var(--muted); line-height: 1.65; transform: translateZ(35px); }

    .card-big { grid-column: span 7; min-height: 380px; }
    .card-medium { grid-column: span 5; min-height: 380px; }
    .card-small { grid-column: span 4; }

    .card-art {
      position: absolute;
      inset: auto 20px 20px auto;
      width: 155px;
      height: 155px;
      border-radius: 34px;
      background: conic-gradient(from var(--angle, 0deg), var(--pink), var(--violet), var(--cyan), var(--lime), var(--pink));
      filter: drop-shadow(0 12px 20px rgba(0,0,0,.3));
      animation: spin 8s linear infinite;
      transform: translateZ(70px);
    }

    .card-big .card-art { width: 250px; height: 250px; right: 32px; bottom: 30px; }

    .mini-orbits {
      position: absolute;
      width: 250px;
      height: 250px;
      right: 42px;
      bottom: 42px;
      border: 1px solid rgba(255,255,255,.18);
      border-radius: 50%;
      animation: spin 9s linear infinite;
    }

    .mini-orbits::before,
    .mini-orbits::after {
      content: "";
      position: absolute;
      width: 18px;
      height: 18px;
      border-radius: 50%;
      background: var(--cyan);
      box-shadow: 0 0 28px var(--cyan);
    }

    .mini-orbits::before { top: -9px; left: 50%; }
    .mini-orbits::after { bottom: 26px; right: 14px; background: var(--pink); box-shadow: 0 0 28px var(--pink); }

    .bubble-field { position: absolute; inset: 0; pointer-events: none; }
    .bubble {
      position: absolute;
      width: var(--s);
      height: var(--s);
      border-radius: 50%;
      background: rgba(255,255,255,.11);
      border: 1px solid rgba(255,255,255,.16);
      animation: bubbleUp var(--d) linear infinite;
      left: var(--x);
      bottom: -40px;
    }

    @keyframes bubbleUp {
      to { transform: translateY(-360px) translateX(var(--move)); opacity: 0; }
    }

    .stats-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 18px;
    }

    .stat-card {
      position: relative;
      padding: 28px;
      min-height: 190px;
      border-radius: var(--radius);
      border: 1px solid var(--border);
      background: rgba(255,255,255,.07);
      overflow: hidden;
    }

    .stat-card::before {
      content: "";
      position: absolute;
      inset: auto -30px -70px auto;
      width: 160px;
      height: 160px;
      border-radius: 50%;
      background: var(--color);
      filter: blur(30px);
      opacity: .5;
    }

    .stat-card b {
      display: block;
      position: relative;
      z-index: 1;
      font-size: clamp(2.4rem, 5vw, 4.6rem);
      letter-spacing: -.08em;
      line-height: .9;
    }

    .stat-card span {
      position: relative;
      z-index: 1;
      display: block;
      margin-top: 14px;
      color: var(--muted);
      line-height: 1.5;
    }

    .gallery {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 18px;
    }

    .tile {
      position: relative;
      min-height: 330px;
      border-radius: 30px;
      overflow: hidden;
      border: 1px solid var(--border);
      background: #111;
      box-shadow: 0 14px 38px rgba(0,0,0,.30);
      transition: transform .24s ease, filter .24s ease;
    }

    .tile.tall { min-height: 430px; }
    .tile.wide { grid-column: span 2; }

    .tile:hover {
      transform: translateY(-10px) scale(1.02);
      filter: saturate(1.25);
    }

    .tile::before {
      content: "";
      position: absolute;
      inset: 0;
      background:
        radial-gradient(circle at 20% 20%, var(--a), transparent 11rem),
        radial-gradient(circle at 80% 40%, var(--b), transparent 12rem),
        linear-gradient(140deg, var(--c), #07070e);
      transition: transform .5s ease;
    }

    .tile:hover::before { transform: scale(1.12) rotate(4deg); }

    .tile::after {
      content: "";
      position: absolute;
      inset: 18px;
      border-radius: 24px;
      border: 1px solid rgba(255,255,255,.18);
      background:
        linear-gradient(135deg, rgba(255,255,255,.16), transparent 40%),
        repeating-linear-gradient(45deg, rgba(255,255,255,.08) 0 1px, transparent 1px 14px);
      opacity: .75;
      transition: opacity .25s ease, transform .25s ease;
    }

    .tile:hover::after { opacity: 1; transform: scale(.96); }

    .tile-info {
      position: absolute;
      left: 18px;
      right: 18px;
      bottom: 18px;
      z-index: 2;
      padding: 16px;
      border-radius: 22px;
      border: 1px solid rgba(255,255,255,.16);
      background: rgba(5,5,10,.58);
      backdrop-filter: blur(16px);
      transform: translateY(8px);
      transition: transform .25s ease;
    }

    .tile:hover .tile-info { transform: translateY(0); }
    .tile-info small { color: var(--cyan); font-weight: 900; letter-spacing: .08em; text-transform: uppercase; }
    .tile-info h3 { margin-top: 7px; }

    .story {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 18px;
      align-items: start;
    }

    .sticky-copy {
      position: sticky;
      top: 115px;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      background: rgba(255,255,255,.07);
      padding: 32px;
    }

    .sticky-copy h2 {
      font-size: clamp(2.2rem, 5vw, 4.8rem);
      line-height: .92;
      letter-spacing: -.08em;
      margin-bottom: 18px;
    }

    .sticky-copy p {
      color: var(--muted);
      line-height: 1.75;
    }

    .timeline {
      display: grid;
      gap: 18px;
    }

    .timeline-item {
      min-height: 250px;
      padding: 28px;
      border-radius: var(--radius);
      border: 1px solid var(--border);
      background:
        radial-gradient(circle at 85% 15%, var(--glow), transparent 12rem),
        rgba(255,255,255,.07);
      transform: translateX(0);
      transition: transform .25s ease, border-color .25s ease;
    }

    .timeline-item:hover {
      transform: translateX(-10px);
      border-color: rgba(255,255,255,.36);
    }

    .timeline-item small {
      color: var(--cyan);
      text-transform: uppercase;
      font-weight: 900;
      letter-spacing: .08em;
    }

    .timeline-item h3 {
      font-size: 1.7rem;
      margin: 12px 0;
    }

    .timeline-item p {
      color: var(--muted);
      line-height: 1.7;
    }

    .carousel-wrap {
      position: relative;
      overflow: hidden;
      border-radius: var(--radius);
      border: 1px solid var(--border);
      background: rgba(255,255,255,.055);
      padding: 18px;
    }

    .carousel {
      display: flex;
      gap: 18px;
      transition: transform .45s cubic-bezier(.16,1,.3,1);
    }

    .slide {
      flex: 0 0 calc(33.333% - 12px);
      min-height: 340px;
      padding: 26px;
      border-radius: 26px;
      border: 1px solid rgba(255,255,255,.14);
      background:
        radial-gradient(circle at 80% 20%, var(--glow), transparent 12rem),
        rgba(255,255,255,.075);
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      transition: transform .25s ease;
    }

    .slide:hover { transform: translateY(-8px); }
    .slide .num {
      font-size: 4rem;
      font-weight: 950;
      letter-spacing: -.09em;
      color: transparent;
      -webkit-text-stroke: 1px rgba(255,255,255,.42);
    }

    .slide h3 {
      margin-bottom: 10px;
      font-size: 1.45rem;
    }

    .slide p {
      color: var(--muted);
      line-height: 1.65;
    }

    .carousel-controls {
      display: flex;
      justify-content: end;
      gap: 10px;
      margin-top: 16px;
    }

    .icon-btn {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      border: 1px solid var(--border);
      background: rgba(255,255,255,.08);
      color: white;
      font-size: 1.3rem;
      transition: transform .2s ease, background .2s ease;
    }

    .icon-btn:hover {
      transform: translateY(-3px);
      background: rgba(255,255,255,.15);
    }

    .lab {
      display: grid;
      grid-template-columns: .9fr 1.1fr;
      gap: 18px;
      align-items: stretch;
    }

    .panel {
      border: 1px solid var(--border);
      border-radius: var(--radius);
      background: rgba(255,255,255,.07);
      padding: 28px;
      overflow: hidden;
    }

    .panel h3 { font-size: 1.6rem; margin-bottom: 10px; }
    .panel p { color: var(--muted); line-height: 1.65; margin-bottom: 20px; }

    .palette {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 12px;
    }

    .swatch {
      height: 95px;
      border: 1px solid rgba(255,255,255,.18);
      border-radius: 22px;
      background: var(--color);
      transition: transform .18s ease, border-radius .18s ease;
    }

    .swatch:hover {
      transform: translateY(-8px) rotate(-3deg);
      border-radius: 50%;
    }

    .click-area {
      position: relative;
      min-height: 310px;
      border: 1px dashed rgba(255,255,255,.25);
      border-radius: 26px;
      display: grid;
      place-items: center;
      color: var(--muted);
      overflow: hidden;
      user-select: none;
    }

    .spark {
      position: absolute;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      pointer-events: none;
      background: var(--spark);
      box-shadow: 0 0 24px var(--spark);
      animation: spark .75s ease-out forwards;
    }

    @keyframes spark {
      to { opacity: 0; transform: translate(var(--dx), var(--dy)) scale(.1); }
    }

    .mixer {
      display: grid;
      grid-template-columns: 1fr 1.3fr;
      gap: 18px;
      align-items: stretch;
    }

    .controls {
      display: grid;
      gap: 14px;
    }

    .control {
      padding: 18px;
      border-radius: 22px;
      border: 1px solid rgba(255,255,255,.14);
      background: rgba(255,255,255,.06);
    }

    .control label {
      display: flex;
      justify-content: space-between;
      color: var(--muted);
      font-weight: 800;
      margin-bottom: 12px;
    }

    input[type="range"] {
      width: 100%;
      accent-color: var(--cyan);
    }

    .mixer-preview {
      position: relative;
      min-height: 420px;
      border-radius: var(--radius);
      border: 1px solid var(--border);
      background:
        radial-gradient(circle at 25% 30%, hsl(var(--hue), 90%, 64%), transparent calc(var(--blur) * 1rem)),
        radial-gradient(circle at 75% 70%, hsl(calc(var(--hue) + 110), 90%, 62%), transparent calc(var(--blur) * 1rem)),
        rgba(255,255,255,.06);
      overflow: hidden;
    }

    .mixer-shape {
      position: absolute;
      left: 50%;
      top: 50%;
      width: var(--size);
      height: var(--size);
      border-radius: var(--round);
      transform: translate(-50%, -50%) rotate(var(--rot));
      background: conic-gradient(from 90deg, hsl(var(--hue), 90%, 64%), hsl(calc(var(--hue) + 90), 90%, 64%), hsl(calc(var(--hue) + 190), 90%, 64%), hsl(var(--hue), 90%, 64%));
      box-shadow: 0 18px 48px rgba(0,0,0,.38);
      transition: .16s linear;
      animation: breathe 3.8s ease-in-out infinite;
    }

    @keyframes breathe {
      50% { transform: translate(-50%, -50%) rotate(calc(var(--rot) + 12deg)) scale(1.08); }
    }

    .poster-wall {
      display: grid;
      grid-template-columns: repeat(6, 1fr);
      gap: 14px;
    }

    .poster {
      min-height: 240px;
      border-radius: 26px;
      border: 1px solid var(--border);
      overflow: hidden;
      position: relative;
      background:
        radial-gradient(circle at var(--px, 50%) var(--py, 30%), rgba(255,255,255,.26), transparent 9rem),
        linear-gradient(145deg, var(--p1), var(--p2));
      transition: transform .2s ease;
    }

    .poster:hover {
      transform: translateY(-8px) rotate(var(--r));
    }

    .poster::before {
      content: "";
      position: absolute;
      inset: 16px;
      border: 1px solid rgba(255,255,255,.22);
      border-radius: 20px;
      background:
        linear-gradient(130deg, rgba(255,255,255,.18), transparent 45%),
        repeating-linear-gradient(-45deg, rgba(255,255,255,.08) 0 1px, transparent 1px 10px);
    }

    .poster:nth-child(2n) { transform: translateY(18px); }
    .poster:nth-child(2n):hover { transform: translateY(8px) rotate(var(--r)); }

    .contact {
      display: grid;
      grid-template-columns: .85fr 1.15fr;
      gap: 18px;
    }

    .contact-card {
      border: 1px solid var(--border);
      border-radius: var(--radius);
      background: rgba(255,255,255,.07);
      padding: 30px;
    }

    .contact-card h3 {
      font-size: 1.8rem;
      margin-bottom: 12px;
    }

    .contact-card p {
      color: var(--muted);
      line-height: 1.75;
    }

    form {
      display: grid;
      gap: 14px;
    }

    input,
    textarea {
      width: 100%;
      padding: 16px 18px;
      border-radius: 18px;
      border: 1px solid var(--border);
      background: rgba(255,255,255,.065);
      color: white;
      font: inherit;
      outline: none;
      transition: border-color .2s ease, background .2s ease, transform .2s ease;
    }

    input:focus,
    textarea:focus {
      border-color: rgba(255,255,255,.44);
      background: rgba(255,255,255,.1);
      transform: translateY(-2px);
    }

    textarea {
      min-height: 150px;
      resize: vertical;
    }

    .form-note {
      min-height: 24px;
      color: var(--cyan);
      font-weight: 800;
    }

    .footer {
      padding: 40px 0 58px;
      color: var(--muted);
      text-align: center;
    }

    .reveal {
      opacity: 0;
      transform: translateY(34px);
      transition: opacity .75s ease, transform .75s ease;
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }


    /* Performance layer */
    section.reveal,
    #liczby,
    #galeria,
    #story,
    #karuzela,
    #lab,
    #mixer,
    #postery,
    #kontakt {
      content-visibility: auto;
      contain-intrinsic-size: 900px;
    }

    .is-idle .orb,
    .is-idle .blob,
    .is-idle .ticker-track,
    .is-idle .sound-bars span,
    .is-idle .card-art,
    .is-idle .mini-orbits,
    .is-idle .bubble {
      animation-play-state: paused;
    }

    .perf-mode .trail {
      display: none;
    }

    .perf-mode .orb {
      opacity: .22;
      filter: blur(22px);
    }

    .perf-mode body::after {
      animation: none;
      opacity: .22;
    }

    .perf-mode .noise {
      display: none;
    }

    .perf-mode .blob,
    .perf-mode .card-art,
    .perf-mode .mini-orbits {
      animation-duration: 12s;
    }

    .perf-mode .interactive-card,
    .perf-mode .tile,
    .perf-mode .poster,
    .perf-mode .slide,
    .perf-mode .stat-card,
    .perf-mode .timeline-item,
    .perf-mode .panel,
    .perf-mode .contact-card {
      box-shadow: none !important;
    }

    @media (max-width: 960px), (pointer: coarse) {
      .trail,
      .noise {
        display: none;
      }

      .orb {
        filter: blur(20px);
        opacity: .24;
      }

      body::after {
        animation: none;
        opacity: .22;
      }

      .blob,
      .card-art,
      .mini-orbits,
      .ticker-track {
        animation-duration: 18s;
      }

      .interactive-card,
      .tile,
      .poster,
      .slide,
      .stat-card,
      .timeline-item,
      .panel,
      .contact-card {
        box-shadow: none !important;
      }
    }

    @media (prefers-reduced-motion: reduce) {
      *,
      *::before,
      *::after {
        animation-duration: .001ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: .001ms !important;
        scroll-behavior: auto !important;
      }

      .trail,
      .cursor,
      .cursor-dot {
        display: none !important;
      }

      body, a, button, input, textarea, .clickable {
        cursor: auto !important;
      }
    }

    @media (max-width: 1050px) {
      .nav-links a:nth-child(n+5) { display: none; }
      .poster-wall { grid-template-columns: repeat(3, 1fr); }
      .stats-grid { grid-template-columns: repeat(2, 1fr); }
    }

    @media (max-width: 960px) {
      body, a, button, input, textarea, .clickable { cursor: auto; }
      .cursor, .cursor-dot { display: none; }

      .hero-grid,
      .lab,
      .story,
      .mixer,
      .contact {
        grid-template-columns: 1fr;
      }

      .sticky-copy {
        position: relative;
        top: 0;
      }

      .stage { height: 500px; }

      .playground,
      .gallery {
        grid-template-columns: 1fr 1fr;
      }

      .card-big,
      .card-medium,
      .card-small,
      .tile.wide {
        grid-column: span 1;
      }

      .nav-links {
        display: none;
      }

      .slide {
        flex-basis: calc(50% - 9px);
      }
    }

    @media (max-width: 620px) {
      .container,
      .nav { width: calc(100% - 28px); }

      .hero { padding-top: 110px; }
      .stage { height: 430px; border-radius: 30px; }

      .blob.one { left: 35px; top: 75px; }
      .blob.two { right: 36px; }
      .blob.three { width: 190px; height: 190px; }

      .glass-card {
        left: 22px;
        bottom: 22px;
        width: calc(100% - 44px);
      }

      .section-head {
        flex-direction: column;
        align-items: start;
      }

      .playground,
      .gallery,
      .palette,
      .poster-wall,
      .stats-grid {
        grid-template-columns: 1fr;
      }

      .slide {
        flex-basis: 100%;
      }

      section { padding: 76px 0; }
    }
  </style>
</head>
<body>
  <div class="progress" id="progress"></div>
  <div class="noise"></div>
  <div class="cursor" id="cursor"></div>
  <div class="cursor-dot" id="cursorDot"></div>

  <div class="orb one"></div>
  <div class="orb two"></div>
  <div class="orb three"></div>
  <div class="orb four"></div>

  <header class="nav">
    <a class="brand magnetic" href="#top">
      <span>✦</span>
      <span>Creative Playground XL</span>
    </a>

    <nav class="nav-links">
      <a class="magnetic" href="#interakcje">Interakcje</a>
      <a class="magnetic" href="#liczby">Liczby</a>
      <a class="magnetic" href="#galeria">Galeria</a>
      <a class="magnetic" href="#story">Story</a>
      <a class="magnetic" href="#mixer">Mixer</a>
      <a class="magnetic" href="#kontakt">Kontakt</a>
      <button class="tiny-btn magnetic" id="randomTheme">Losuj kolor</button>
      <button class="tiny-btn magnetic" id="perfToggle">Tryb wydajności</button>
    </nav>
  </header>

  <main id="top">
    <section class="hero">
      <div class="container hero-grid">
        <div>
          <div class="tag"><i></i> Najedź, klikaj, przesuwaj, scrolluj</div>
          <h1>
            Strona, która <span class="outline">żyje</span> pod kursorem.
          </h1>
          <p>
            Dłuższa wersja kreatywnego playgroundu: kursor z trailami, 3D tilt, hover glow,
            karuzela, sticky story, interaktywny mixer, click sparks, poster wall i animowane statystyki.
          </p>
          <div class="actions">
            <a class="btn primary magnetic" href="#interakcje">Start interakcji</a>
            <a class="btn magnetic" href="#mixer">Pobaw się mixerem</a>
          </div>
        </div>

        <div class="stage tilt">
          <div class="blob one"></div>
          <div class="blob two"></div>
          <div class="blob three"></div>
          <div class="blob four"></div>

          <div class="sound-bars" aria-hidden="true">
            <span style="--h:32"></span>
            <span style="--h:48"></span>
            <span style="--h:22"></span>
            <span style="--h:40"></span>
            <span style="--h:28"></span>
          </div>

          <div class="glass-card">
            <h3>Interactive motion system</h3>
            <p>Porusz myszką po tej karcie. Stage obraca się w 3D, a cały background reaguje na pozycję kursora.</p>
          </div>
        </div>
      </div>
    </section>

    <div class="ticker">
      <div class="ticker-track">
        <span>HOVER EFFECTS ✦ 3D CARDS ✦ CURSOR TRAILS ✦ MIXER ✦ CLICK SPARKS ✦ POSTER WALL ✦</span>
        <span>HOVER EFFECTS ✦ 3D CARDS ✦ CURSOR TRAILS ✦ MIXER ✦ CLICK SPARKS ✦ POSTER WALL ✦</span>
      </div>
    </div>

    <section class="reveal" id="interakcje">
      <div class="container">
        <div class="section-head">
          <h2>Interaktywne karty</h2>
          <p>
            Najedź na karty. Każda reaguje na pozycję kursora, ma światło pod myszką i delikatny obrót 3D.
          </p>
        </div>

        <div class="playground">
          <article class="interactive-card card-big tilt spotlight">
            <h3>3D hover canvas</h3>
            <p>Ruch kursora zmienia kąt nachylenia karty i pozycję światła. Efekt dobrze wygląda w portfolio, dashboardzie lub stronie kreatywnej.</p>
            <div class="mini-orbits"></div>
          </article>

          <article class="interactive-card card-medium tilt spotlight">
            <h3>Liquid shape</h3>
            <p>Animowane kształty organiczne tworzą wrażenie żywej, płynnej grafiki.</p>
            <div class="card-art"></div>
          </article>

          <article class="interactive-card card-small tilt spotlight">
            <h3>Glow UI</h3>
            <p>Przyjemny glassmorphism, światło i miękkie przejścia.</p>
            <div class="card-art" style="width: 110px; height: 110px;"></div>
          </article>

          <article class="interactive-card card-small tilt spotlight">
            <h3>Motion mood</h3>
            <p>Dużo ruchu, ale nadal czytelny układ strony.</p>
            <div class="bubble-field">
              <span class="bubble" style="--s:22px; --x:18%; --d:5s; --move:25px;"></span>
              <span class="bubble" style="--s:34px; --x:46%; --d:7s; --move:-18px;"></span>
              <span class="bubble" style="--s:18px; --x:72%; --d:4.8s; --move:12px;"></span>
              <span class="bubble" style="--s:26px; --x:84%; --d:6.2s; --move:-28px;"></span>
            </div>
          </article>

          <article class="interactive-card card-small tilt spotlight">
            <h3>Micro feedback</h3>
            <p>Przyciski i linki mają efekt magnetyczny oraz światło pod kursorem.</p>
            <div class="card-art" style="width: 95px; height: 95px; border-radius: 50%;"></div>
          </article>
        </div>
      </div>
    </section>

    <section class="reveal" id="liczby">
      <div class="container">
        <div class="section-head">
          <h2>Animowane liczby</h2>
          <p>
            Po doscrollowaniu liczby odpalają animację. To fajne do pokazania skilli, projektów, klientów albo statystyk.
          </p>
        </div>

        <div class="stats-grid">
          <div class="stat-card spotlight" style="--color: var(--pink);">
            <b class="count" data-target="92">0</b>
            <span>% vibe'u wizualnego i efektów hover</span>
          </div>
          <div class="stat-card spotlight" style="--color: var(--cyan);">
            <b class="count" data-target="48">0</b>
            <span>mikrointerakcji, przejść i animowanych elementów</span>
          </div>
          <div class="stat-card spotlight" style="--color: var(--lime);">
            <b class="count" data-target="12">0</b>
            <span>sekcji i bloków do rozbudowania w portfolio</span>
          </div>
          <div class="stat-card spotlight" style="--color: var(--orange);">
            <b class="count" data-target="100">0</b>
            <span>% gotowe do dalszego personalizowania</span>
          </div>
        </div>
      </div>
    </section>

    <section class="reveal" id="galeria">
      <div class="container">
        <div class="section-head">
          <h2>Żywa galeria</h2>
          <p>Więcej kafelków, różne wysokości i szerokości. Każdy ma własny gradient i hover zoom.</p>
        </div>

        <div class="gallery">
          <article class="tile tall" style="--a:#ff4ecd; --b:#35e7ff; --c:#21102c;">
            <div class="tile-info"><small>Poster</small><h3>Neon Bloom</h3></div>
          </article>
          <article class="tile" style="--a:#8f5cff; --b:#c6ff4e; --c:#111b2c;">
            <div class="tile-info"><small>Avatar</small><h3>Cyber Mask</h3></div>
          </article>
          <article class="tile" style="--a:#ffb14e; --b:#ff4ecd; --c:#2b1710;">
            <div class="tile-info"><small>Banner</small><h3>Heat Wave</h3></div>
          </article>
          <article class="tile tall" style="--a:#35e7ff; --b:#8f5cff; --c:#0d1c24;">
            <div class="tile-info"><small>Brand</small><h3>Liquid ID</h3></div>
          </article>
          <article class="tile wide" style="--a:#c6ff4e; --b:#35e7ff; --c:#102018;">
            <div class="tile-info"><small>Experiment</small><h3>Acid Interface</h3></div>
          </article>
          <article class="tile" style="--a:#ff4e6a; --b:#ffb14e; --c:#2b1015;">
            <div class="tile-info"><small>Social</small><h3>Motion Pack</h3></div>
          </article>
          <article class="tile" style="--a:#ffffff; --b:#8f5cff; --c:#15131f;">
            <div class="tile-info"><small>Cover</small><h3>White Pulse</h3></div>
          </article>
        </div>
      </div>
    </section>

    <section class="reveal" id="story">
      <div class="container story">
        <div class="sticky-copy">
          <h2>Sticky story section</h2>
          <p>
            Ta sekcja robi stronę dłuższą i bardziej filmową. Lewa karta zostaje przyklejona podczas scrollowania,
            a po prawej pojawiają się kolejne etapy procesu kreatywnego.
          </p>
        </div>

        <div class="timeline">
          <article class="timeline-item reveal" style="--glow: rgba(255,78,205,.28);">
            <small>01 — Idea</small>
            <h3>Najpierw klimat</h3>
            <p>Ustalasz vibe: neon, cyber, soft, dark, gaming, portfolio albo artystyczny chaos. Kolory robią pierwsze wrażenie.</p>
          </article>

          <article class="timeline-item reveal" style="--glow: rgba(53,231,255,.28);">
            <small>02 — Layout</small>
            <h3>Potem rytm strony</h3>
            <p>Duże nagłówki, kontrastowe karty, przyjemne odstępy i sekcje, które naturalnie prowadzą użytkownika dalej.</p>
          </article>

          <article class="timeline-item reveal" style="--glow: rgba(198,255,78,.23);">
            <small>03 — Motion</small>
            <h3>Na końcu satysfakcja</h3>
            <p>Hover, tilt, glow, trail, scroll reveal i klikane efekty dodają stronie feelingu, przez który chce się ją eksplorować.</p>
          </article>

          <article class="timeline-item reveal" style="--glow: rgba(255,177,78,.25);">
            <small>04 — Polish</small>
            <h3>Drobne detale</h3>
            <p>Najlepsze strony często wygrywają detalami: reakcją kursora, mikroanimacją, miękką zmianą koloru albo ruchem tła.</p>
          </article>
        </div>
      </div>
    </section>

    <section class="reveal" id="karuzela">
      <div class="container">
        <div class="section-head">
          <h2>Karuzela pomysłów</h2>
          <p>Przesuwaj kartami. Każdy slajd można przerobić na usługę, projekt, case study albo sekcję oferty.</p>
        </div>

        <div class="carousel-wrap">
          <div class="carousel" id="carousel">
            <article class="slide" style="--glow: rgba(255,78,205,.28);">
              <div class="num">01</div>
              <div><h3>Avatar design</h3><p>Stylizowane ikony profilowe, gamingowe profile, social media i Roblox vibe.</p></div>
            </article>
            <article class="slide" style="--glow: rgba(53,231,255,.26);">
              <div class="num">02</div>
              <div><h3>Banner pack</h3><p>Banery na YouTube, Discord, Twitter/X, profile i strony społecznościowe.</p></div>
            </article>
            <article class="slide" style="--glow: rgba(198,255,78,.22);">
              <div class="num">03</div>
              <div><h3>Digital artwork</h3><p>Grafiki klimatyczne, plakaty, okładki i kompozycje pod własny styl.</p></div>
            </article>
            <article class="slide" style="--glow: rgba(255,177,78,.25);">
              <div class="num">04</div>
              <div><h3>Photo edit</h3><p>Kolor, retusz, światło, efekty, manipulacje i poprawa wizualnego charakteru.</p></div>
            </article>
            <article class="slide" style="--glow: rgba(143,92,255,.3);">
              <div class="num">05</div>
              <div><h3>Brand mood</h3><p>Kolory, typografia, mini system wizualny i spójny klimat marki/profilu.</p></div>
            </article>
          </div>

          <div class="carousel-controls">
            <button class="icon-btn magnetic" id="prevSlide">‹</button>
            <button class="icon-btn magnetic" id="nextSlide">›</button>
          </div>
        </div>
      </div>
    </section>

    <section class="reveal" id="lab">
      <div class="container">
        <div class="section-head">
          <h2>Creative Lab</h2>
          <p>Kliknij w pole po prawej, żeby wywołać iskry. Po lewej najedź na kolory — zmieniają kształt jak żelki.</p>
        </div>

        <div class="lab">
          <div class="panel">
            <h3>Palette candy</h3>
            <p>Kolory można łatwo zmienić w CSS. To dobra baza pod portfolio, stronę eventową albo interaktywną prezentację.</p>
            <div class="palette">
              <div class="swatch" style="--color:#ff4ecd"></div>
              <div class="swatch" style="--color:#8f5cff"></div>
              <div class="swatch" style="--color:#35e7ff"></div>
              <div class="swatch" style="--color:#c6ff4e"></div>
              <div class="swatch" style="--color:#ffb14e"></div>
            </div>
          </div>

          <div class="panel">
            <h3>Click sparks</h3>
            <p>Kliknij kilka razy w pole. Ten efekt można potem wykorzystać np. przy przycisku, galerii albo ekranie startowym.</p>
            <div class="click-area clickable" id="clickArea">Kliknij tutaj ✦</div>
          </div>
        </div>
      </div>
    </section>

    <section class="reveal" id="mixer">
      <div class="container">
        <div class="section-head">
          <h2>Shape mixer</h2>
          <p>Interaktywne suwaki zmieniają obiekt na żywo. To mały kreatywny generator w środku strony.</p>
        </div>

        <div class="mixer">
          <div class="panel controls">
            <div class="control">
              <label><span>Kolor</span><span id="hueValue">280</span></label>
              <input id="hueRange" type="range" min="0" max="360" value="280">
            </div>
            <div class="control">
              <label><span>Rozmiar</span><span id="sizeValue">210</span></label>
              <input id="sizeRange" type="range" min="120" max="320" value="210">
            </div>
            <div class="control">
              <label><span>Zaokrąglenie</span><span id="roundValue">34</span></label>
              <input id="roundRange" type="range" min="0" max="50" value="34">
            </div>
            <div class="control">
              <label><span>Glow</span><span id="blurValue">18</span></label>
              <input id="blurRange" type="range" min="8" max="28" value="18">
            </div>
          </div>

          <div class="mixer-preview" id="mixerPreview" style="--hue:280; --size:210px; --round:34%; --blur:18; --rot:0deg;">
            <div class="mixer-shape"></div>
          </div>
        </div>
      </div>
    </section>

    <section class="reveal" id="postery">
      <div class="container">
        <div class="section-head">
          <h2>Poster wall</h2>
          <p>Ściana małych plakatów. Najedź na nie — każdy lekko się obraca, a światło w środku podąża za kursorem.</p>
        </div>

        <div class="poster-wall">
          <div class="poster spotlight" style="--p1:#ff4ecd;--p2:#15101f;--r:-4deg;"></div>
          <div class="poster spotlight" style="--p1:#35e7ff;--p2:#101d26;--r:3deg;"></div>
          <div class="poster spotlight" style="--p1:#8f5cff;--p2:#171027;--r:-2deg;"></div>
          <div class="poster spotlight" style="--p1:#c6ff4e;--p2:#101f16;--r:4deg;"></div>
          <div class="poster spotlight" style="--p1:#ffb14e;--p2:#25160e;--r:-5deg;"></div>
          <div class="poster spotlight" style="--p1:#ff4e6a;--p2:#240f15;--r:2deg;"></div>
        </div>
      </div>
    </section>

    <section class="reveal" id="kontakt">
      <div class="container">
        <div class="section-head">
          <h2>Kontakt / CTA</h2>
          <p>Na końcu długiej strony można dać prosty formularz, link do portfolio albo zachętę do współpracy.</p>
        </div>

        <div class="contact">
          <div class="contact-card spotlight">
            <h3>Masz pomysł na klimat?</h3>
            <p>
              Ten plik możesz potraktować jako bazę do naprawdę odjechanego portfolio. Podmień teksty, kolory,
              projekty, dodaj swoje grafiki i masz stronę, która od razu robi wrażenie.
            </p>
          </div>

          <div class="contact-card">
            <form id="contactForm">
              <input type="text" name="name" placeholder="Twoje imię" required>
              <input type="email" name="email" placeholder="Twój email" required>
              <input type="text" name="topic" placeholder="Temat projektu">
              <textarea name="message" placeholder="Opisz projekt albo efekt, jaki chcesz zrobić..." required></textarea>
              <button class="btn primary magnetic" type="submit">Wyślij wiadomość</button>
              <div class="form-note" id="formNote"></div>
            </form>
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer class="footer">
    <div class="container">
      Creative Playground XL ✦ HTML + CSS + JavaScript ✦ Naciśnij G, żeby zmienić klimat kolorów
    </div>
  </footer>

  <script>
    const root = document.documentElement;
    const progress = document.getElementById("progress");
    const cursor = document.getElementById("cursor");
    const cursorDot = document.getElementById("cursorDot");
    const perfToggle = document.getElementById("perfToggle");

    const coarsePointer = window.matchMedia("(pointer: coarse)").matches;
    const reducedMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;

    let perfMode = localStorage.getItem("creativePerfMode") === "1" || coarsePointer || reducedMotion;
    if (perfMode) document.documentElement.classList.add("perf-mode");

    let mouseX = window.innerWidth / 2;
    let mouseY = window.innerHeight / 2;
    let cursorX = mouseX;
    let cursorY = mouseY;
    let lastTrail = 0;
    let lastBgUpdate = 0;
    let tickingProgress = false;
    let idleTimer;

    function setIdleState() {
      document.documentElement.classList.remove("is-idle");
      clearTimeout(idleTimer);
      idleTimer = setTimeout(() => document.documentElement.classList.add("is-idle"), 3500);
    }

    window.addEventListener("mousemove", event => {
      mouseX = event.clientX;
      mouseY = event.clientY;
      setIdleState();

      const now = performance.now();

      if (now - lastBgUpdate > 40) {
        root.style.setProperty("--mx", `${mouseX}px`);
        root.style.setProperty("--my", `${mouseY}px`);
        lastBgUpdate = now;
      }

      cursorDot.style.transform = `translate(${mouseX}px, ${mouseY}px) translate(-50%, -50%)`;

      if (!perfMode && !coarsePointer && window.innerWidth > 960 && now - lastTrail > 85) {
        const trail = document.createElement("span");
        trail.className = "trail";
        trail.style.transform = `translate(${mouseX}px, ${mouseY}px) translate(-50%, -50%)`;
        document.body.appendChild(trail);
        setTimeout(() => trail.remove(), 720);
        lastTrail = now;
      }
    }, { passive: true });

    function animateCursor() {
      cursorX += (mouseX - cursorX) * 0.16;
      cursorY += (mouseY - cursorY) * 0.16;
      cursor.style.transform = `translate(${cursorX}px, ${cursorY}px) translate(-50%, -50%)`;
      requestAnimationFrame(animateCursor);
    }

    if (!reducedMotion && !coarsePointer) animateCursor();

    function updateProgress() {
      const scrollable = document.documentElement.scrollHeight - window.innerHeight;
      const ratio = scrollable > 0 ? window.scrollY / scrollable : 0;
      progress.style.transform = `scaleX(${ratio})`;
      tickingProgress = false;
    }

    window.addEventListener("scroll", () => {
      setIdleState();
      if (!tickingProgress) {
        requestAnimationFrame(updateProgress);
        tickingProgress = true;
      }
    }, { passive: true });
    updateProgress();

    perfToggle.addEventListener("click", () => {
      perfMode = !perfMode;
      document.documentElement.classList.toggle("perf-mode", perfMode);
      localStorage.setItem("creativePerfMode", perfMode ? "1" : "0");
      perfToggle.textContent = perfMode ? "Pełne efekty" : "Tryb wydajności";
    });

    perfToggle.textContent = perfMode ? "Pełne efekty" : "Tryb wydajności";

    document.querySelectorAll("a, button, .tile, .interactive-card, .swatch, .poster, input, textarea, .click-area").forEach(el => {
      el.addEventListener("mouseenter", () => cursor.classList.add("big"));
      el.addEventListener("mouseleave", () => cursor.classList.remove("big"));
    });

    document.querySelectorAll(".btn").forEach(btn => {
      btn.addEventListener("mousemove", event => {
        const rect = btn.getBoundingClientRect();
        btn.style.setProperty("--bx", `${event.clientX - rect.left}px`);
        btn.style.setProperty("--by", `${event.clientY - rect.top}px`);
      });
    });

    document.querySelectorAll(".spotlight").forEach(card => {
      let raf = null;
      let lastEvent = null;

      card.addEventListener("mousemove", event => {
        if (perfMode) return;
        lastEvent = event;
        if (raf) return;

        raf = requestAnimationFrame(() => {
          const rect = card.getBoundingClientRect();
          const x = lastEvent.clientX - rect.left;
          const y = lastEvent.clientY - rect.top;
          card.style.setProperty("--cx", `${x}px`);
          card.style.setProperty("--cy", `${y}px`);
          card.style.setProperty("--px", `${x}px`);
          card.style.setProperty("--py", `${y}px`);
          raf = null;
        });
      }, { passive: true });
    });

    document.querySelectorAll(".tilt").forEach(card => {
      let raf = null;
      let lastEvent = null;

      card.addEventListener("mousemove", event => {
        if (perfMode || window.innerWidth <= 960) return;

        lastEvent = event;
        if (raf) return;

        raf = requestAnimationFrame(() => {
          const rect = card.getBoundingClientRect();
          const x = lastEvent.clientX - rect.left;
          const y = lastEvent.clientY - rect.top;
          const rotateY = ((x / rect.width) - 0.5) * 10;
          const rotateX = ((y / rect.height) - 0.5) * -10;

          card.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) translateY(-3px)`;
          raf = null;
        });
      }, { passive: true });

      card.addEventListener("mouseleave", () => {
        card.style.transform = "perspective(1000px) rotateX(0deg) rotateY(0deg) translateY(0)";
      });
    });

    document.querySelectorAll(".magnetic").forEach(el => {
      let raf = null;
      let lastEvent = null;

      el.addEventListener("mousemove", event => {
        if (perfMode || window.innerWidth <= 960) return;

        lastEvent = event;
        if (raf) return;

        raf = requestAnimationFrame(() => {
          const rect = el.getBoundingClientRect();
          const x = lastEvent.clientX - rect.left - rect.width / 2;
          const y = lastEvent.clientY - rect.top - rect.height / 2;
          el.style.transform = `translate(${x * 0.14}px, ${y * 0.14}px)`;
          raf = null;
        });
      }, { passive: true });

      el.addEventListener("mouseleave", () => {
        el.style.transform = "translate(0,0)";
      });
    });

    const revealObserver = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.isIntersecting) entry.target.classList.add("visible");
      });
    }, { threshold: 0.16 });

    document.querySelectorAll(".reveal").forEach(el => revealObserver.observe(el));

    const countObserver = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (!entry.isIntersecting || entry.target.dataset.done) return;

        entry.target.dataset.done = "true";
        const target = Number(entry.target.dataset.target);
        let current = 0;
        const duration = 1100;
        const start = performance.now();

        function tick(now) {
          const progress = Math.min((now - start) / duration, 1);
          const eased = 1 - Math.pow(1 - progress, 3);
          current = Math.round(target * eased);
          entry.target.textContent = current;
          if (progress < 1) requestAnimationFrame(tick);
        }

        requestAnimationFrame(tick);
      });
    }, { threshold: 0.5 });

    document.querySelectorAll(".count").forEach(el => countObserver.observe(el));

    const clickArea = document.getElementById("clickArea");
    const sparkColors = ["#ff4ecd", "#8f5cff", "#35e7ff", "#c6ff4e", "#ffb14e"];

    clickArea.addEventListener("click", event => {
      const rect = clickArea.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      for (let i = 0; i < (perfMode ? 8 : 18); i++) {
        const spark = document.createElement("span");
        spark.className = "spark";
        const angle = Math.random() * Math.PI * 2;
        const distance = 42 + Math.random() * 115;
        const dx = Math.cos(angle) * distance;
        const dy = Math.sin(angle) * distance;

        spark.style.left = `${x}px`;
        spark.style.top = `${y}px`;
        spark.style.setProperty("--dx", `${dx}px`);
        spark.style.setProperty("--dy", `${dy}px`);
        spark.style.setProperty("--spark", sparkColors[Math.floor(Math.random() * sparkColors.length)]);

        clickArea.appendChild(spark);
        setTimeout(() => spark.remove(), 780);
      }
    });

    const carousel = document.getElementById("carousel");
    const prevSlide = document.getElementById("prevSlide");
    const nextSlide = document.getElementById("nextSlide");
    let slideIndex = 0;

    function visibleSlides() {
      if (window.innerWidth <= 620) return 1;
      if (window.innerWidth <= 960) return 2;
      return 3;
    }

    function updateCarousel() {
      const slides = document.querySelectorAll(".slide");
      const max = Math.max(0, slides.length - visibleSlides());
      slideIndex = Math.max(0, Math.min(slideIndex, max));
      const slideWidth = slides[0].getBoundingClientRect().width + 18;
      carousel.style.transform = `translateX(${-slideIndex * slideWidth}px)`;
    }

    prevSlide.addEventListener("click", () => {
      slideIndex--;
      updateCarousel();
    });

    nextSlide.addEventListener("click", () => {
      slideIndex++;
      updateCarousel();
    });

    window.addEventListener("resize", updateCarousel);
    updateCarousel();

    const mixerPreview = document.getElementById("mixerPreview");
    const hueRange = document.getElementById("hueRange");
    const sizeRange = document.getElementById("sizeRange");
    const roundRange = document.getElementById("roundRange");
    const blurRange = document.getElementById("blurRange");
    const hueValue = document.getElementById("hueValue");
    const sizeValue = document.getElementById("sizeValue");
    const roundValue = document.getElementById("roundValue");
    const blurValue = document.getElementById("blurValue");

    function updateMixer() {
      mixerPreview.style.setProperty("--hue", hueRange.value);
      mixerPreview.style.setProperty("--size", `${sizeRange.value}px`);
      mixerPreview.style.setProperty("--round", `${roundRange.value}%`);
      mixerPreview.style.setProperty("--blur", blurRange.value);
      mixerPreview.style.setProperty("--rot", `${Number(hueRange.value) / 2}deg`);

      hueValue.textContent = hueRange.value;
      sizeValue.textContent = sizeRange.value;
      roundValue.textContent = roundRange.value;
      blurValue.textContent = blurRange.value;
    }

    [hueRange, sizeRange, roundRange, blurRange].forEach(input => {
      input.addEventListener("input", updateMixer);
    });
    updateMixer();

    const randomTheme = document.getElementById("randomTheme");

    function setRandomTheme() {
      const base = Math.floor(Math.random() * 360);

      root.style.setProperty("--pink", `hsl(${base}, 95%, 64%)`);
      root.style.setProperty("--violet", `hsl(${(base + 55) % 360}, 95%, 66%)`);
      root.style.setProperty("--cyan", `hsl(${(base + 135) % 360}, 95%, 62%)`);
      root.style.setProperty("--lime", `hsl(${(base + 205) % 360}, 95%, 64%)`);
      root.style.setProperty("--orange", `hsl(${(base + 295) % 360}, 95%, 64%)`);
    }

    randomTheme.addEventListener("click", setRandomTheme);

    window.addEventListener("keydown", event => {
      if (event.key.toLowerCase() === "g") {
        setRandomTheme();
      }
    });

    const form = document.getElementById("contactForm");
    const note = document.getElementById("formNote");

    form.addEventListener("submit", event => {
      event.preventDefault();

      const data = new FormData(form);
      const name = data.get("name");
      const email = data.get("email");
      const topic = data.get("topic") || "Creative portfolio";
      const message = data.get("message");

      note.textContent = "Wiadomość przygotowana — podmień email w kodzie na swój.";
      const mailto = `mailto:twoj-email@example.com?subject=${encodeURIComponent(topic)}&body=${encodeURIComponent(
        `Imię: ${name}\nEmail: ${email}\n\nWiadomość:\n${message}`
      )}`;

      setTimeout(() => {
        window.location.href = mailto;
      }, 300);
    });
  </script>
</body>
</html>
