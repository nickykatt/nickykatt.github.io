---
layout: page
title: Tulse Hill → London Bridge
permalink: /trains/
description: Live departures from Tulse Hill to London Bridge, refreshed every 30 seconds.
---

{% raw %}
<style>
  .board { background:#111; color:#ffb000; font-family:"Courier New",ui-monospace,monospace; border-radius:8px; padding:16px; margin:1em 0; }
  .board h2 { color:#ffb000; font-size:1.1em; margin:0 0 12px; letter-spacing:.05em; display:flex; justify-content:space-between; gap:8px; flex-wrap:wrap; }
  .board .clock { color:#fff; }
  .board table { width:100%; border-collapse:collapse; font-size:.95em; }
  .board th { text-align:left; color:#888; font-weight:normal; font-size:.8em; text-transform:uppercase; padding:4px 6px; border-bottom:1px solid #333; }
  .board td { padding:8px 6px; border-bottom:1px solid #222; vertical-align:top; }
  .board .dest small { display:block; color:#999; font-size:.75em; }
  .board .ontime { color:#5fd35f; }
  .board .late { color:#ff6b6b; }
  .board .cancelled td { color:#ff6b6b; text-decoration:line-through; }
  .board .cancelled td.exp { text-decoration:none; }
  .board .msg { color:#ccc; font-size:.85em; margin-top:10px; }
  .board .status { color:#777; font-size:.75em; margin-top:10px; }
  @media (max-width:480px){ .board .hide-sm{ display:none; } .board td,.board th{ padding:6px 3px; } }
</style>

<div class="board" aria-live="polite">
  <h2><span>TULSE HILL → LONDON BRIDGE</span><span class="clock" id="clock"></span></h2>
  <table>
    <thead>
      <tr><th>Due</th><th>Destination</th><th>Plat</th><th>Expected</th><th class="hide-sm">Arr LBG</th></tr>
    </thead>
    <tbody id="rows"><tr><td colspan="5">Loading…</td></tr></tbody>
  </table>
  <div class="msg" id="messages"></div>
  <div class="status" id="status"></div>
</div>

<p><small>Live data from National Rail Enquiries via the open <a href="https://github.com/jpsingleton/Huxley2">Huxley 2</a> proxy. Refreshes every 30 seconds.</small></p>

<script>
(function () {
  var FROM = "TUH", TO = "LBG", ROWS = 10, REFRESH_MS = 30000;
  var HOSTS = ["https://huxley2.azurewebsites.net"];

  function esc(s) {
    return String(s == null ? "" : s).replace(/[&<>"']/g, function (c) {
      return { "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" }[c];
    });
  }

  function tick() {
    document.getElementById("clock").textContent =
      new Date().toLocaleTimeString("en-GB", { timeZone: "Europe/London" });
  }

  function arrivalAt(svc, crs) {
    var lists = svc.subsequentCallingPoints || [];
    for (var i = 0; i < lists.length; i++) {
      var pts = lists[i].callingPoint || [];
      for (var j = 0; j < pts.length; j++) {
        if (pts[j].crs === crs) {
          var p = pts[j];
          return (p.et && p.et !== "On time" && p.et !== "Cancelled") ? p.et : p.st;
        }
      }
    }
    return "";
  }

  function render(data) {
    var svcs = data.trainServices || [];
    var tbody = document.getElementById("rows");
    if (!svcs.length) {
      tbody.innerHTML = '<tr><td colspan="5">No direct trains to London Bridge in the next couple of hours.</td></tr>';
    } else {
      tbody.innerHTML = svcs.map(function (s) {
        var dest = (s.destination || []).map(function (d) {
          return esc(d.locationName) + (d.via ? " <small>" + esc(d.via) + "</small>" : "");
        }).join(" &amp; ");
        var etd = s.etd || "";
        var cls = etd === "On time" ? "ontime" : "late";
        var cancelled = s.isCancelled || etd === "Cancelled";
        return '<tr class="' + (cancelled ? "cancelled" : "") + '">' +
          "<td>" + esc(s.std) + "</td>" +
          '<td class="dest">' + dest + "<small>" + esc(s.operator) + "</small></td>" +
          "<td>" + esc(s.platform || "–") + "</td>" +
          '<td class="exp ' + cls + '">' + esc(etd) + "</td>" +
          '<td class="hide-sm">' + esc(cancelled ? "" : arrivalAt(s, TO)) + "</td>" +
          "</tr>";
      }).join("");
    }
    var msgs = (data.nrccMessages || []).map(function (m) {
      var txt = (m.value || "").replace(/<[^>]*>/g, "").trim();
      return txt ? "⚠ " + esc(txt) : "";
    }).filter(Boolean);
    document.getElementById("messages").innerHTML = msgs.join("<br>");
    document.getElementById("status").textContent =
      "Updated " + new Date().toLocaleTimeString("en-GB", { timeZone: "Europe/London" });
  }

  function load(i) {
    i = i || 0;
    var url = HOSTS[i] + "/departures/" + FROM + "/to/" + TO + "/" + ROWS + "?expand=true";
    fetch(url, { cache: "no-store" })
      .then(function (r) { if (!r.ok) throw new Error(r.status); return r.json(); })
      .then(render)
      .catch(function () {
        if (i + 1 < HOSTS.length) return load(i + 1);
        document.getElementById("status").textContent =
          "Couldn't reach the live feed just now — retrying in 30s.";
      });
  }

  tick(); setInterval(tick, 1000);
  load(); setInterval(load, REFRESH_MS);
})();
</script>
{% endraw %}
