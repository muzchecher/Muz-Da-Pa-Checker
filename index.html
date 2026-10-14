<?php
/* ===== MUZCHECKERPRO – single file site =====
   SETUP: paste your Moz API keys below (https://moz.com/products/api), upload this
   file to any PHP host as index.php, and open your domain. Nothing else is needed. */
$id     = getenv('MOZ_ID')     ?: 'YOUR_MOZ_ACCESS_ID';   // <-- paste Access ID
$secret = getenv('MOZ_SECRET') ?: 'YOUR_MOZ_SECRET_KEY';  // <-- paste Secret Key

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
header('Content-Type: application/json');
header('Cache-Control: no-store');
// Muzcheckerpro – live Moz Links API proxy with caching and per-visitor rate limit.

define('CACHE_TTL', 86400); // reuse a result for 24h (saves your Moz quota)
define('HOURLY_CAP', 100); // URLs per visitor IP per hour
define('MAX_PER_REQ', 50);  // Moz limit

function fail($code, $msg) { http_response_code($code); echo json_encode(['error' => $msg]); exit; }
if (strpos($id, 'YOUR_') === 0) fail(503, 'Service is not configured yet (Moz API keys missing).');

$in = json_decode(file_get_contents('php://input'), true);
$urls = [];
foreach (($in['urls'] ?? []) as $u) {
  $u = trim($u); if ($u === '') continue;
  $u = preg_replace('~^https?://~i', '', $u); $u = rtrim($u, '/');
  if (!preg_match('~^[a-z0-9.-]+\.[a-z]{2,}(/.*)?$~i', $u)) continue; // skip invalid input
  $urls[strtolower($u)] = true;
}
$urls = array_slice(array_keys($urls), 0, MAX_PER_REQ);
if (!$urls) fail(400, 'No valid URLs found. Enter domains like example.com.');

// Per-IP hourly limit
$dir = sys_get_temp_dir() . '/muzchecker'; @mkdir($dir, 0700, true);
$ipf = "$dir/ip_" . md5($_SERVER['REMOTE_ADDR'] ?? 'x') . '_' . date('YmdH');
$used = (int)@file_get_contents($ipf);
if ($used + count($urls) > HOURLY_CAP) fail(429, 'Hourly limit reached. Try again later.');
file_put_contents($ipf, $used + count($urls));

// Cache lookup
$rows = []; $todo = [];
foreach ($urls as $u) {
  $cf = "$dir/c_" . md5($u);
  if (is_file($cf) && time() - filemtime($cf) < CACHE_TTL) $rows[$u] = json_decode(file_get_contents($cf), true);
  else $todo[] = $u;
}

if ($todo) {
  $ch = curl_init('https://lsapi.seomoz.com/v2/url_metrics');
  curl_setopt_array($ch, [
    CURLOPT_POST => true, CURLOPT_RETURNTRANSFER => true, CURLOPT_TIMEOUT => 30,
    CURLOPT_USERPWD => "$id:$secret",
    CURLOPT_HTTPHEADER => ['Content-Type: application/json'],
    CURLOPT_POSTFIELDS => json_encode(['targets' => $todo]),
  ]);
  $res = curl_exec($ch); $code = curl_getinfo($ch, CURLINFO_HTTP_CODE); curl_close($ch);
  if ($code !== 200) fail(502, $code === 401 || $code === 403 ? 'Moz API keys rejected.' : "Moz API error ($code). Try again.");
  foreach ((json_decode($res, true)['results'] ?? []) as $i => $r) {
    $u = $todo[$i] ?? null; if (!$u) continue;
    $row = [
      'url' => $u,
      'da'  => $r['domain_authority'] ?? null,
      'pa'  => $r['page_authority'] ?? null,
      'ss'  => $r['spam_score'] ?? null,
      'linking_domains' => $r['root_domains_to_root_domain'] ?? null,
      'inbound_links'   => $r['external_pages_to_root_domain'] ?? null,
    ];
    $rows[$u] = $row;
    file_put_contents("$dir/c_" . md5($u), json_encode($row));
  }
}
$out = []; foreach ($urls as $u) if (isset($rows[$u])) $out[] = $rows[$u];
echo json_encode(['results' => $out]);
exit;
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Muzcheckerpro – Bulk Domain Authority, Page Authority &amp; Spam Score</title>
<meta name="description" content="Check Domain Authority, Page Authority and Spam Score for up to 50 URLs at once. Filter results and export to CSV.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@500;700;800&family=Source+Sans+3:wght@400;600&display=swap" rel="stylesheet">
<style>
:root{--bg:#f6f7f4;--surface:#fff;--ink:#14201c;--muted:#5d6b66;--line:#dde3df;--brand:#0b6b57;--brand-ink:#fff;--good:#0b6b57;--mid:#b7791f;--bad:#b3261e;--r:10px}
@media (prefers-color-scheme:dark){:root:not([data-theme=light]){--bg:#0f1714;--surface:#16211d;--ink:#e8efec;--muted:#9fb0a9;--line:#27362f;--brand:#3fbf9f;--brand-ink:#08130f;--good:#3fbf9f;--mid:#e0a94a;--bad:#ff8a80}}
*{box-sizing:border-box}
html{height:100%}
body{margin:0;background:var(--bg);color:var(--ink);font:17px/1.6 "Source Sans 3",system-ui,sans-serif;padding-top:env(safe-area-inset-top,0)}
h1,h2,h3{font-family:"Bricolage Grotesque",system-ui,sans-serif;line-height:1.15;margin:0 0 .5em}
.wrap{max-width:1040px;margin:0 auto;padding:0 20px}
header{border-bottom:1px solid var(--line)}
header .wrap{display:flex;justify-content:space-between;align-items:center;height:60px}
.logo{font:800 20px "Bricolage Grotesque",sans-serif;letter-spacing:-.02em;text-decoration:none;color:var(--ink)}
.logo b{color:var(--brand)}
nav a{color:var(--muted);text-decoration:none;margin-left:20px;font-weight:600;font-size:15px}
nav a:hover{color:var(--ink)}
.hero{padding:44px 0 20px}
.hero h1{font-size:clamp(32px,6vw,54px);font-weight:800;letter-spacing:-.03em;max-width:15em}
.hero p{color:var(--muted);max-width:40em;margin:0}
.tool{background:var(--surface);border:1px solid var(--line);border-radius:var(--r);padding:20px;margin:24px 0}
textarea{width:100%;min-height:170px;resize:vertical;border:1px solid var(--line);border-radius:8px;background:var(--bg);color:var(--ink);padding:14px;font:15px/1.5 ui-monospace,Menlo,Consolas,monospace}
textarea:focus,input:focus,button:focus-visible{outline:3px solid var(--brand);outline-offset:1px}
.bar{display:flex;flex-wrap:wrap;gap:10px;align-items:center;margin-top:12px}
.count{color:var(--muted);font-size:15px;margin-right:auto}
button{font:600 16px inherit;font-family:inherit;border-radius:8px;padding:11px 20px;cursor:pointer;border:1px solid var(--line);background:transparent;color:var(--ink)}
button.primary{background:var(--brand);color:var(--brand-ink);border-color:var(--brand)}
button:disabled{opacity:.55;cursor:wait}
.filters{display:none;gap:16px;flex-wrap:wrap;margin-top:16px;padding-top:16px;border-top:1px solid var(--line);font-size:15px}
.filters.on{display:flex}
.filters label{display:flex;gap:8px;align-items:center;color:var(--muted)}
.filters input[type=number]{width:72px;padding:6px 8px;border:1px solid var(--line);border-radius:6px;background:var(--bg);color:var(--ink)}
.msg{margin-top:12px;font-size:15px;color:var(--muted)}
.msg.err{color:var(--bad)}
.tablebox{overflow-x:auto;margin-top:20px;border:1px solid var(--line);border-radius:8px}
table{border-collapse:collapse;width:100%;min-width:640px;font-size:15px}
th,td{padding:11px 14px;text-align:right;border-bottom:1px solid var(--line);white-space:nowrap}
th:first-child,td:first-child{text-align:left;max-width:300px;overflow:hidden;text-overflow:ellipsis}
th{cursor:pointer;user-select:none;background:var(--bg);font-weight:600;color:var(--muted)}
th[aria-sort=ascending]::after{content:" ▲"}th[aria-sort=descending]::after{content:" ▼"}
tr:last-child td{border-bottom:0}
.pill{display:inline-block;min-width:42px;padding:2px 10px;border-radius:99px;font-weight:700;text-align:center;border:1.5px solid currentColor}
.good{color:var(--good)}.mid{color:var(--mid)}.bad{color:var(--bad)}
.empty{padding:28px;text-align:center;color:var(--muted)}
.info{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:32px;padding:32px 0 20px}
.info p{color:var(--muted);margin:0}
.faq{padding:10px 0 48px;max-width:46em}
details{border-top:1px solid var(--line);padding:14px 0}
summary{font-weight:600;cursor:pointer}
details p{color:var(--muted);margin:8px 0 0}
footer{border-top:1px solid var(--line);padding:24px 0;color:var(--muted);font-size:14px}
</style>
</head>
<body>
<header><div class="wrap">
  <a class="logo" href="#">Muz<b>checker</b>pro</a>
  <nav><a href="#tool">Checker</a><a href="#faq">FAQ</a></nav>
</div></header>

<main class="wrap">
  <section class="hero">
    <h1>Check Domain Authority, Page Authority and Spam Score in bulk</h1>
    <p>Paste up to 50 URLs, one per line. Filter by score and export the results to CSV.</p>
  </section>

  <section class="tool" id="tool" aria-label="Bulk checker">
    <label for="urls" class="msg" style="display:block;margin:0 0 8px">URLs or domains, one per line</label>
    <textarea id="urls" placeholder="example.com&#10;https://another-site.org/blog/post"></textarea>
    <div class="bar">
      <span class="count" id="count">0 / 50 URLs</span>
      <button id="filterBtn" type="button" aria-expanded="false">Filters</button>
      <button id="clear" type="button">Clear</button>
      <button id="run" class="primary" type="button">Check DA PA</button>
    </div>
    <div class="filters" id="filters">
      <label>DA above <input type="number" id="fDA" min="0" max="100" value="0"></label>
      <label>PA above <input type="number" id="fPA" min="0" max="100" value="0"></label>
      <label>Spam score below <input type="number" id="fSS" min="0" max="100" value="100"></label>
    </div>
    <div class="msg" id="msg" role="status"></div>
    <div class="tablebox">
      <table id="tbl">
        <thead><tr>
          <th data-k="url">URL</th><th data-k="da">DA</th><th data-k="pa">PA</th><th data-k="ss">Spam score</th><th data-k="ld">Linking domains</th><th data-k="il">Inbound links</th>
        </tr></thead>
        <tbody id="rows"><tr><td colspan="6" class="empty">Results appear here after you run a check.</td></tr></tbody>
      </table>
    </div>
    <div class="bar"><span class="count"></span><button id="csv" type="button" disabled>Download CSV</button></div>
  </section>

  <section class="info">
    <div><h3>Domain Authority (DA)</h3><p>A 1–100 score from Moz that estimates how well a whole domain may rank. It is relative: compare it with sites in your own niche.</p></div>
    <div><h3>Page Authority (PA)</h3><p>The same 1–100 idea for a single page. Use it to find which pages on a site carry the most link strength.</p></div>
    <div><h3>Spam Score (SS)</h3><p>The share of sites with similar features that Moz has seen penalized. Lower is safer; above 30% deserves a manual look.</p></div>
  </section>

  <section class="faq" id="faq">
    <h2>Questions</h2>
    <details><summary>Does Google use DA or PA?</summary><p>No. They are Moz metrics. They are useful for comparing sites and vetting link sources, but they are not Google ranking factors.</p></details>
    <details><summary>What is a good DA?</summary><p>It depends on your competitors. Above 50 is generally strong, but the right target is whatever the top results in your niche have.</p></details>
    <details><summary>How often should I check?</summary><p>Moz updates its index regularly. Monthly checks are enough to see real movement.</p></details>
  </section>
</main>
<footer><div class="wrap">© Muzcheckerpro. Metrics provided by the Moz Links API. DA, PA and Spam Score are trademarks of Moz.</div></footer>

<script>
// ===== Settings =====
const API_URL = location.pathname; // posts to this same file
const DEMO_MODE = false; // live mode: real Moz data only
const MAX_URLS = 50;
// ====================
const $ = id => document.getElementById(id);
let data = [], sortKey = null, sortDir = 1;

const parse = () => [...new Set($("urls").value.split(/\r?\n/).map(s => s.trim()).filter(Boolean))];
const update = () => { const n = parse().length; $("count").textContent = n + " / " + MAX_URLS + " URLs"; $("count").style.color = n > MAX_URLS ? "var(--bad)" : ""; };
$("urls").addEventListener("input", update);

$("filterBtn").onclick = () => { const on = $("filters").classList.toggle("on"); $("filterBtn").setAttribute("aria-expanded", on); };
["fDA","fPA","fSS"].forEach(id => $(id).addEventListener("input", render));
$("clear").onclick = () => { $("urls").value = ""; data = []; $("msg").textContent = ""; $("csv").disabled = true; update(); render(); };

// Deterministic fake numbers so the demo is stable. Not real metrics.
function demo(u) {
  let h = 2166136261; for (const c of u.toLowerCase().replace(/^https?:\/\/(www\.)?/, "")) h = Math.imul(h ^ c.charCodeAt(0), 16777619) >>> 0;
  const r = n => { h = Math.imul(h ^ (h >>> 15), 2246822507) >>> 0; return h % n; };
  const da = 10 + r(85);
  return { url: u, da, pa: Math.max(1, da - r(18)), ss: r(40), linking_domains: r(da * 400), inbound_links: r(da * 9000) };
}

$("run").onclick = async () => {
  const urls = parse();
  if (!urls.length) { $("msg").className = "msg err"; $("msg").textContent = "Add at least one URL."; return; }
  if (urls.length > MAX_URLS) { $("msg").className = "msg err"; $("msg").textContent = "Limit is " + MAX_URLS + " URLs per check. Remove " + (urls.length - MAX_URLS) + "."; return; }
  $("run").disabled = true; $("run").textContent = "Checking…"; $("msg").className = "msg"; $("msg").textContent = "";
  try {
    if (DEMO_MODE) { await new Promise(r => setTimeout(r, 600)); data = urls.map(demo); $("msg").textContent = "Demo mode: these numbers are sample data, not real Moz metrics."; }
    else {
      const res = await fetch(API_URL, { method: "POST", headers: { "Content-Type": "application/json" }, body: JSON.stringify({ urls }) });
      const j = await res.json();
      if (!res.ok) throw new Error(j.error || "Request failed");
      data = j.results;
    }
    $("csv").disabled = !data.length; render();
  } catch (e) {
    $("msg").className = "msg err"; $("msg").textContent = "Check failed: " + e.message;
  }
  $("run").disabled = false; $("run").textContent = "Check DA PA";
};

const cls = (v, lo, hi, inv) => v == null ? "" : (inv ? (v < lo ? "good" : v < hi ? "mid" : "bad") : (v >= hi ? "good" : v >= lo ? "mid" : "bad"));
const pill = (v, lo, hi, inv) => v == null ? "–" : `<span class="pill ${cls(v, lo, hi, inv)}">${Math.round(v)}${inv ? "%" : ""}</span>`;
const esc = s => String(s).replace(/[&<>"]/g, c => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;" }[c]));

function view() {
  const a = +$("fDA").value || 0, p = +$("fPA").value || 0, s = $("fSS").value === "" ? 100 : +$("fSS").value;
  let v = data.filter(d => (d.da ?? 0) > a && (d.pa ?? 0) > p && (d.ss ?? 0) < s);
  if (sortKey) { const m = { url: "url", da: "da", pa: "pa", ss: "ss", ld: "linking_domains", il: "inbound_links" }[sortKey];
    v.sort((x, y) => (sortKey === "url" ? String(x.url).localeCompare(y.url) : (x[m] ?? -1) - (y[m] ?? -1)) * sortDir); }
  return v;
}
function render() {
  const v = view();
  $("rows").innerHTML = v.length ? v.map(d => `<tr><td title="${esc(d.url)}">${esc(d.url)}</td><td>${pill(d.da, 30, 50)}</td><td>${pill(d.pa, 30, 50)}</td><td>${pill(d.ss, 10, 30, true)}</td><td>${(d.linking_domains ?? "–").toLocaleString?.() ?? "–"}</td><td>${(d.inbound_links ?? "–").toLocaleString?.() ?? "–"}</td></tr>`).join("")
    : `<tr><td colspan="6" class="empty">${data.length ? "No results match your filters." : "Results appear here after you run a check."}</td></tr>`;
  document.querySelectorAll("th").forEach(th => th.setAttribute("aria-sort", th.dataset.k === sortKey ? (sortDir > 0 ? "ascending" : "descending") : "none"));
}
document.querySelectorAll("th").forEach(th => th.onclick = () => { const k = th.dataset.k; sortDir = sortKey === k ? -sortDir : (k === "url" ? 1 : -1); sortKey = k; render(); });

$("csv").onclick = () => {
  const q = x => '"' + String(x ?? "").replace(/"/g, '""') + '"';
  const out = [["URL","DA","PA","Spam Score","Linking Domains","Inbound Links"], ...view().map(d => [d.url, d.da, d.pa, d.ss, d.linking_domains, d.inbound_links])].map(r => r.map(q).join(",")).join("\n");
  const a = document.createElement("a"); a.href = URL.createObjectURL(new Blob([out], { type: "text/csv" })); a.download = "da-pa-results.csv"; a.click();
};
update();
</script>
</body>
</html>
