---
layout: page
title: Commute
permalink: /trains/
description: Live train departures between Tulse Hill and London Bridge, with a traffic-light guide to when to leave and Jubilee line status.
---

{% raw %}
<style>
  .cb { --ink:#17181A; --muted:#55565A; --soft:#6B6C70; --ground:#F6F5F1; --card:#FFFFFF; --chip:#E6E4DE;
        --green:#17633A; --green-soft:#DDF3E5; --green-light:#C9EED7; --amber:#E89A00; --amber-ink:#8A5A00; --amber-soft:#FFF4DC;
        --red:#B3261E; --red-soft:#FBE4E2;
        background:var(--ground); color:var(--ink); font-family:Inter,system-ui,-apple-system,"Segoe UI",sans-serif;
        border-radius:20px; padding:16px; max-width:480px; margin:0 auto 1.5em; display:flex; flex-direction:column; gap:14px; }
  .cb * { box-sizing:border-box; }
  .cb-top { display:flex; justify-content:space-between; align-items:center; gap:8px; }
  .cb-tabs { display:flex; background:var(--chip); border-radius:999px; padding:4px; }
  .cb-tabs button { height:44px; padding:0 18px; border:0; border-radius:999px; background:transparent; color:var(--muted);
        font:inherit; font-size:14px; cursor:pointer; }
  .cb-tabs button[aria-selected="true"] { background:var(--ink); color:#fff; font-weight:600; }
  .cb-clock { font-size:15px; font-weight:600; color:var(--muted); font-variant-numeric:tabular-nums; }
  .cb-hero { border-radius:20px; padding:20px; display:flex; flex-direction:column; gap:6px; color:#fff; background:var(--green); }
  .cb-hero.amber { background:#8A5A00; }
  .cb-hero.none { background:#3A3B3F; }
  .cb-hero .route { font-size:13px; font-weight:500; opacity:.85; }
  .cb-hero .lead { font-size:15px; }
  .cb-hero .big { font-size:64px; font-weight:800; line-height:1; letter-spacing:-.02em; font-variant-numeric:tabular-nums; }
  .cb-hero .act { font-size:16px; font-weight:600; }
  .cb-hero .meta { font-size:13px; opacity:.85; }
  .cb-list { display:flex; flex-direction:column; gap:8px; }
  .cb-row { background:var(--card); border-radius:14px; padding:14px 16px; display:flex; align-items:center; gap:12px; border:2px solid transparent; }
  .cb-row .dot { width:12px; height:12px; border-radius:50%; flex:none; }
  .cb-row .t { font-size:20px; font-weight:700; font-variant-numeric:tabular-nums; }
  .cb-row .sub { font-size:12px; color:var(--soft); }
  .cb-row .main { flex:1; display:flex; flex-direction:column; }
  .cb-row .st { font-size:14px; font-weight:600; text-align:right; }
  .cb-row.green { background:var(--green-soft); } .cb-row.green .dot { background:var(--green); } .cb-row.green .st { color:var(--green); }
  .cb-row.green.rec { border-color:var(--green); }
  .cb-row.amber .dot { background:var(--amber); } .cb-row.amber .st { color:var(--amber-ink); }
  .cb-row.later .dot { background:#B5B6BA; } .cb-row.later .st { color:var(--soft); }
  .cb-note { font-size:13px; color:var(--soft); padding:2px 4px; line-height:1.5; }
  .cb-tube { background:var(--card); border-radius:14px; padding:12px 16px; display:flex; flex-direction:column; gap:4px; font-size:14px; }
  .cb-tube.warn { background:var(--amber-soft); } .cb-tube.bad { background:var(--red-soft); }
  .cb-tube .head { display:flex; align-items:center; gap:10px; }
  .cb-tube .roundel { width:22px; height:22px; border-radius:50%; border:4px solid #868F98; flex:none; }
  .cb-tube .sev { font-weight:600; color:var(--green); }
  .cb-tube.warn .sev { color:var(--amber-ink); } .cb-tube.bad .sev { color:var(--red); }
  .cb-tube .why { font-size:12px; color:var(--muted); line-height:1.4; }
  .cb-quote { padding:4px 4px 2px; }
  .cb-quote p { margin:0; }
  .cb-quote .q { font-family:Georgia,"Times New Roman",serif; font-style:italic; font-size:19px; line-height:1.4; color:#2A2B2E; }
  .cb-quote .who { margin-top:8px; font-size:13px; color:var(--soft); }
  .cb-status { font-size:11px; color:var(--soft); text-align:center; }
  @media (max-width:380px){ .cb-tabs button{ padding:0 12px; } .cb-hero .big{ font-size:52px; } }
</style>

<div class="cb" id="cb">
  <div class="cb-top">
    <div class="cb-tabs" role="tablist" aria-label="Direction">
      <button role="tab" id="tab-work" aria-selected="true">To work</button>
      <button role="tab" id="tab-home" aria-selected="false">To home</button>
    </div>
    <span class="cb-clock" id="cb-clock"></span>
  </div>
  <div class="cb-hero none" id="cb-hero" aria-live="polite"><span class="route" id="cb-route"></span><span class="lead">Loading live departures…</span></div>
  <div class="cb-list" id="cb-list"></div>
  <div class="cb-note" id="cb-note"></div>
  <div class="cb-tube" id="cb-tube"><div class="head"><span class="roundel"></span><span style="flex:1"><b>Jubilee line</b></span><span class="sev">Checking…</span></div></div>
  <div class="cb-quote" id="cb-quote"></div>
  <div class="cb-status" id="cb-status"></div>
</div>

<p><small>Train data from National Rail Enquiries via the open <a href="https://github.com/jpsingleton/Huxley2">Huxley 2</a> proxy. Tube status from <a href="https://tfl.gov.uk/">Transport for London</a> open data. Times use the expected departure where a train is running late.</small></p>

<script>
(function () {
  var VIEWS = {
    work: { from: "TUH", to: "LBG", route: "Tulse Hill → London Bridge", buffer: 10,
            red: 10, amber: 20, greenMax: 60, leaveWord: "leave the house by" },
    home: { from: "LBG", to: "TUH", route: "London Bridge → Tulse Hill", buffer: 20,
            red: 20, amber: 30, greenMax: Infinity, leaveWord: "leave your desk by" }
  };

  var QUOTES = {
    work: [
      ["No society can legitimately call itself civilised if a sick person is denied medical aid because of lack of means.", "Aneurin Bevan, In Place of Fear (1952)"],
      ["Illness is neither an indulgence for which people have to pay, nor an offence for which they should be penalised, but a misfortune, the cost of which should be shared by the community.", "Aneurin Bevan, In Place of Fear (1952)"],
      ["The NHS belongs to the people.", "The NHS Constitution for England"],
      ["Everyone – rich or poor, man, woman or child – can use it or any part of it.", "Leaflet introducing the new NHS, 1948"],
      ["It may seem a strange principle to enunciate as the very first requirement in a Hospital that it should do the sick no harm.", "Florence Nightingale, Notes on Hospitals"],
      ["Place the quality of patient care, especially patient safety, above all other aims.", "Don Berwick, A promise to learn – a commitment to act (2013)"],
      ["The strategy is delivery.", "Mike Bracken, Government Digital Service"],
      ["Start with user needs.", "GOV.UK design principles"]
    ],
    home: [
      ["The most important thing a father can do for his children is to love their mother.", "Theodore M. Hesburgh"],
      ["Any fool can have a child. That doesn’t make you a father. It’s the courage to raise a child that makes you a father.", "Barack Obama, Father’s Day 2008"],
      ["Love isn’t a state of perfect caring. It is an active noun like ‘struggle’.", "Fred Rogers, The World According to Mister Rogers"],
      ["We’re not raising grass, we’re raising boys.", "Clay Killebrew, as remembered by his son Harmon"],
      ["Call it a clan, call it a network, call it a tribe, call it a family. Whatever you call it, whoever you are, you need one.", "Jane Howard, Families (1978)"]
    ]
  };

  var TRAIN_URL = "https://huxley2.azurewebsites.net/departures/";
  var TUBE_URL = "https://api.tfl.gov.uk/Line/jubilee/Status";

  var view = londonNow().h < 12 ? "work" : "home";
  var data = { work: null, home: null }, err = { work: false, home: false }, updated = { work: null, home: null };

  function $(id) { return document.getElementById(id); }
  function esc(s) {
    return String(s == null ? "" : s).replace(/[&<>"']/g, function (c) {
      return { "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[c];
    });
  }
  function londonNow() {
    var p = new Intl.DateTimeFormat("en-GB", { timeZone: "Europe/London", hour: "2-digit", minute: "2-digit", second: "2-digit", hourCycle: "h23" })
      .formatToParts(new Date()).reduce(function (a, x) { a[x.type] = x.value; return a; }, {});
    return { h: +p.hour, m: +p.minute, s: +p.second, label: p.hour + ":" + p.minute };
  }
  function toMin(hhmm) { var m = /^(\d{1,2}):(\d{2})$/.exec(hhmm || ""); return m ? (+m[1]) * 60 + (+m[2]) : null; }
  function fmt(mins) { mins = ((mins % 1440) + 1440) % 1440; var h = Math.floor(mins / 60), m = Math.round(mins % 60); return (h < 10 ? "0" : "") + h + ":" + (m < 10 ? "0" : "") + m; }

  // Minutes from now until a train's expected departure (handles midnight wrap)
  function minsUntil(depMin) {
    var n = londonNow(), now = n.h * 60 + n.m + n.s / 60, d = depMin - now;
    if (d < -720) d += 1440; else if (d > 720) d -= 1440;
    return d;
  }

  function classify(svc, cfg) {
    var etd = svc.etd || "", sched = toMin(svc.std);
    var cancelled = svc.isCancelled || etd === "Cancelled";
    var exp = toMin(etd); var dep = exp != null ? exp : sched;
    var delayed = etd === "Delayed";
    var mins = minsUntil(dep);
    var band = cancelled ? "cancelled" : mins < 0 ? "gone" : mins < cfg.red ? "red" : mins < cfg.amber ? "amber" : mins <= cfg.greenMax ? "green" : "later";
    return { std: svc.std, dep: dep, depLabel: fmt(dep), late: exp != null && exp !== sched, delayed: delayed,
             mins: mins, band: band, platform: svc.platform, operator: svc.operator };
  }

  function rowHtml(t, rec) {
    var label = t.band === "green" ? "Get ready" : t.band === "amber" ? "Leave now" : "Later";
    var mins = Math.max(0, Math.floor(t.mins));
    var sub = [];
    if (t.late) sub.push("Exp " + t.depLabel + " (sched " + t.std + ")");
    if (t.delayed) sub.push("Delayed");
    sub.push(t.platform ? "Plat " + t.platform : "Plat TBC");
    if (t.operator) sub.push(t.operator);
    return '<div class="cb-row ' + t.band + (rec ? " rec" : "") + '">' +
      '<span class="dot" aria-hidden="true"></span>' +
      '<div class="main"><span class="t">' + esc(t.depLabel) + "</span><span class=\"sub\">" + esc(sub.join(" · ")) + "</span></div>" +
      '<span class="st">' + (rec ? "★ " : "") + label + "<br>" + mins + " min</span></div>";
  }

  function renderTrains() {
    var cfg = VIEWS[view], d = data[view];
    $("cb-clock").textContent = londonNow().label;
    var hero = $("cb-hero"), list = $("cb-list"), note = $("cb-note");
    if (!d) {
      hero.className = "cb-hero none";
      hero.innerHTML = '<span class="route">' + esc(cfg.route) + '</span><span class="lead">' +
        (err[view] ? "Couldn’t reach the live train feed. Retrying…" : "Loading live departures…") + "</span>";
      list.innerHTML = ""; note.textContent = "";
      return;
    }
    var all = (d.trainServices || []).map(function (s) { return classify(s, cfg); })
      .filter(function (t) { return t.band !== "gone"; })
      .sort(function (a, b) { return a.mins - b.mins; });
    var catchable = all.filter(function (t) { return t.band === "amber" || t.band === "green" || t.band === "later"; });
    var top = catchable.slice(0, 3), rest = catchable.slice(3);
    var missed = all.filter(function (t) { return t.band === "red"; });
    var cancelled = all.filter(function (t) { return t.band === "cancelled"; });
    var rec = catchable.filter(function (t) { return t.band === "green"; })[0] || null;
    var lead = rec || catchable[0] || null;

    if (!lead) {
      hero.className = "cb-hero none";
      hero.innerHTML = '<span class="route">' + esc(cfg.route) + '</span><span class="lead">No catchable direct trains in the next two hours.</span>';
    } else {
      var isRec = lead === rec;
      var leaveBy = fmt(lead.dep - cfg.buffer);
      hero.className = "cb-hero " + (lead.band === "amber" ? "amber" : lead.band === "later" ? "none" : "");
      hero.innerHTML = '<span class="route">' + esc(cfg.route) + "</span>" +
        '<span class="lead">' + (lead.band === "amber" ? "Hurry for the" : lead.band === "later" ? "Next good train is the" : "Aim for the") + "</span>" +
        '<span class="big">' + esc(lead.depLabel) + "</span>" +
        '<span class="act">' + (lead.band === "amber" ? "Leave now" : lead.band === "later" ? "No rush yet" : "Get ready") + " · " + cfg.leaveWord + " " + leaveBy + "</span>" +
        '<span class="meta">' + esc((lead.platform ? "Platform " + lead.platform : "Platform TBC") + " · " +
          (lead.late ? "expected " + lead.depLabel + " (sched " + lead.std + ")" : lead.delayed ? "delayed" : "on time") + " · " +
          Math.max(0, Math.floor(lead.mins)) + " min away") + "</span>";
    }

    list.innerHTML = top.map(function (t) { return rowHtml(t, t === rec); }).join("");

    var bits = [];
    if (missed.length) bits.push((view === "work" ? "Just missed: " : "Too late to make: ") +
      missed.map(function (t) { return t.depLabel + " (" + Math.max(0, Math.floor(t.mins)) + " min)"; }).join(", "));
    if (cancelled.length) bits.push("Cancelled: " + cancelled.map(function (t) { return t.std; }).join(", "));
    if (rest.length) bits.push("Later: " + rest.map(function (t) { return t.depLabel; }).join(", "));
    var msgs = (d.nrccMessages || []).map(function (m) { return (m.value || "").replace(/<[^>]*>/g, "").trim(); }).filter(Boolean);
    note.innerHTML = bits.map(esc).join("<br>") + (msgs.length ? (bits.length ? "<br>" : "") + "⚠ " + msgs.map(esc).join("<br>⚠ ") : "");

    $("cb-status").textContent = updated[view] ? "Trains updated " + updated[view] + " · refreshes every 30s" : "";
  }

  function renderQuote() {
    var list = QUOTES[view];
    var day = Math.floor(Date.now() / 86400000);
    var q = list[day % list.length];
    $("cb-quote").innerHTML = '<p class="q">“' + esc(q[0]) + '”</p><p class="who">' + esc(q[1]) + "</p>";
  }

  function loadTrains(v) {
    var cfg = VIEWS[v];
    return fetch(TRAIN_URL + cfg.from + "/to/" + cfg.to + "/15?expand=false", { cache: "no-store" })
      .then(function (r) { if (!r.ok) throw new Error(r.status); return r.json(); })
      .then(function (j) { data[v] = j; err[v] = false; updated[v] = londonNow().label; })
      .catch(function () { err[v] = true; })
      .then(function () { if (v === view) renderTrains(); });
  }

  function loadTube() {
    fetch(TUBE_URL, { cache: "no-store" })
      .then(function (r) { if (!r.ok) throw new Error(r.status); return r.json(); })
      .then(function (j) {
        var statuses = (j[0] && j[0].lineStatuses) || [];
        var worst = statuses.slice().sort(function (a, b) { return a.statusSeverity - b.statusSeverity; })[0] || { statusSeverity: 10, statusSeverityDescription: "Good Service" };
        var sev = worst.statusSeverity;
        var cls = sev >= 10 ? "" : (sev >= 9 || sev === 18 || sev === 19) ? "warn" : "bad";
        var reasons = statuses.map(function (s) { return s.reason; }).filter(Boolean);
        $("cb-tube").className = "cb-tube " + cls;
        $("cb-tube").innerHTML = '<div class="head"><span class="roundel" aria-hidden="true"></span><span style="flex:1"><b>Jubilee line</b></span>' +
          '<span class="sev">' + esc(worst.statusSeverityDescription) + "</span></div>" +
          (reasons.length ? '<span class="why">' + esc(reasons[0]) + "</span>" : "");
      })
      .catch(function () {
        $("cb-tube").innerHTML = '<div class="head"><span class="roundel" aria-hidden="true"></span><span style="flex:1"><b>Jubilee line</b></span><span class="sev" style="color:#6B6C70">Status unavailable</span></div>';
      });
  }

  function setView(v) {
    view = v;
    $("tab-work").setAttribute("aria-selected", v === "work");
    $("tab-home").setAttribute("aria-selected", v === "home");
    renderTrains(); renderQuote();
    if (!data[v]) loadTrains(v);
  }

  $("tab-work").addEventListener("click", function () { setView("work"); });
  $("tab-home").addEventListener("click", function () { setView("home"); });

  setView(view);
  loadTrains("work"); loadTrains("home"); loadTube();
  setInterval(renderTrains, 5000);                    // live countdowns and colour changes
  setInterval(function () { loadTrains(view); }, 30000);
  setInterval(loadTube, 120000);
})();
</script>
{% endraw %}
