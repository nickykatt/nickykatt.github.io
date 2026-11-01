---
layout: page
title: NHS idiom generator
permalink: /idioms/
description: Random made-up idioms for life in NHS digital, each with an explanation.
---

Following on from [idiom bingo]({{ site.baseurl }}{% post_url 2026-09-11-idiom-bingo %}), here are some idioms I've made up for life in NHS digital. Press the button for another.

<div class="idiom-gen">
  <div class="idiom-card" aria-live="polite">
    <p class="idiom-text" id="idiom-text">🐘</p>
    <p class="idiom-meaning" id="idiom-meaning">Press the button for an idiom.</p>
  </div>
  <div class="idiom-actions">
    <button type="button" id="idiom-next">Another one</button>
    <button type="button" id="idiom-copy">Copy</button>
    <span class="idiom-count" id="idiom-count"></span>
  </div>
</div>

<noscript>
<ul>
{%- for i in site.data.idioms %}
  <li><strong>{{ i.idiom }}</strong>: {{ i.meaning }}</li>
{%- endfor %}
</ul>
</noscript>

<style>
  .idiom-gen { margin: 1.5rem 0 2rem; }
  .idiom-card {
    border: 1px solid rgba(128, 128, 128, 0.35);
    border-radius: 8px;
    padding: 1.5rem;
    min-height: 9rem;
  }
  .idiom-text {
    font-size: 1.6rem;
    font-weight: 600;
    line-height: 1.25;
    margin-bottom: 0.75rem;
  }
  .idiom-meaning { margin-bottom: 0; }
  .idiom-actions {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 0.5rem;
    margin-top: 1rem;
  }
  .idiom-actions button {
    font: inherit;
    color: inherit;
    background: transparent;
    border: 1px solid rgba(128, 128, 128, 0.6);
    border-radius: 6px;
    padding: 0.4rem 0.9rem;
    cursor: pointer;
  }
  .idiom-actions button:hover { border-color: currentColor; }
  .idiom-count { opacity: 0.7; font-size: 0.9rem; }
  .idiom-card.fade { animation: idiom-fade 0.3s ease; }
  @keyframes idiom-fade { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: none; } }
  @media (prefers-reduced-motion: reduce) { .idiom-card.fade { animation: none; } }
</style>

<script>
  (function () {
    var idioms = {{ site.data.idioms | jsonify }};
    var textEl = document.getElementById("idiom-text");
    var meaningEl = document.getElementById("idiom-meaning");
    var countEl = document.getElementById("idiom-count");
    var card = textEl.parentElement;
    var bag = [];
    var seen = 0;
    var current = null;

    function refill() {
      bag = idioms.slice();
      for (var i = bag.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var t = bag[i]; bag[i] = bag[j]; bag[j] = t;
      }
      // Avoid showing the same idiom twice in a row across refills
      if (current && bag.length > 1 && bag[bag.length - 1].idiom === current.idiom) {
        bag.unshift(bag.pop());
      }
    }

    function next() {
      if (!bag.length) refill();
      current = bag.pop();
      seen++;
      textEl.textContent = "“" + current.idiom + "”";
      meaningEl.textContent = current.meaning;
      countEl.textContent = seen <= idioms.length
        ? seen + " of " + idioms.length
        : "You've seen them all. Going round again";
      card.classList.remove("fade");
      void card.offsetWidth;
      card.classList.add("fade");
    }

    document.getElementById("idiom-next").addEventListener("click", next);
    document.getElementById("idiom-copy").addEventListener("click", function (e) {
      if (!current) return;
      var btn = e.currentTarget;
      var text = "“" + current.idiom + "”: " + current.meaning;
      var done = function () { btn.textContent = "Copied"; setTimeout(function () { btn.textContent = "Copy"; }, 1500); };
      if (navigator.clipboard) {
        navigator.clipboard.writeText(text).then(done, function () {});
      }
    });

    next();
  })();
</script>
