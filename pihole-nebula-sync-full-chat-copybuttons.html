<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Pi-hole v6: Nebula Sync (DHCP + Unbound) — Full Setup Notes</title>
  <style>
    :root {
      --bg: #0b1020;
      --panel: #111a33;
      --text: #e6edf3;
      --muted: #a9b4c2;
      --accent: #7aa2f7;
      --ok: #2ea043;
      --bad: #d1242f;
      --warn: #d29922;
      --code-bg: #0b1020;
      --code-border: rgba(230,237,243,0.12);
      --shadow: 0 10px 30px rgba(0,0,0,0.35);
    }
    body {
      margin: 0;
      padding: 24px;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, sans-serif;
      background: linear-gradient(180deg, #0b1020, #080b16 60%, #070a14);
      color: var(--text);
      line-height: 1.5;
    }
    .container { max-width: 980px; margin: 0 auto; }
    h1 { font-size: 1.55rem; margin: 0 0 10px 0; }
    h2 { font-size: 1.15rem; margin: 28px 0 10px 0; }
    h3 { font-size: 1.0rem; margin: 18px 0 8px 0; }
    p { margin: 10px 0; color: var(--text); }
    .muted { color: var(--muted); }
    .badge { display:inline-block; padding: 2px 8px; border-radius: 999px; background: rgba(122,162,247,0.15); color: var(--accent); font-size: 0.85rem; }
    .card {
      background: rgba(17,26,51,0.72);
      border: 1px solid rgba(230,237,243,0.10);
      border-radius: 14px;
      padding: 16px 16px;
      box-shadow: var(--shadow);
      margin: 14px 0;
    }
    .callout {
      border-left: 4px solid var(--warn);
      padding: 10px 12px;
      background: rgba(210,153,34,0.10);
      border-radius: 10px;
      margin: 12px 0;
    }
    .ok {
      border-left: 4px solid var(--ok);
      background: rgba(46,160,67,0.10);
    }
    .bad {
      border-left: 4px solid var(--bad);
      background: rgba(209,36,47,0.10);
    }
    details {
      background: rgba(17,26,51,0.55);
      border: 1px solid rgba(230,237,243,0.10);
      border-radius: 12px;
      padding: 10px 12px;
      margin: 10px 0;
    }
    summary {
      cursor: pointer;
      font-weight: 600;
      color: var(--text);
      outline: none;
      list-style: none;
    }
    summary::-webkit-details-marker { display: none; }
    summary:before {
      content: '▸';
      display: inline-block;
      margin-right: 8px;
      color: var(--accent);
      transform: translateY(-1px);
    }
    details[open] summary:before { content: '▾'; }

    pre {
      position: relative;
      margin: 12px 0;
      padding: 14px;
      background: var(--code-bg);
      border: 1px solid var(--code-border);
      border-radius: 12px;
      overflow: auto;
    }
    code { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace; font-size: 0.92rem; }

    .copy-btn {
      position: absolute;
      top: 10px;
      right: 10px;
      border: 1px solid rgba(230,237,243,0.18);
      background: rgba(230,237,243,0.08);
      color: var(--text);
      border-radius: 10px;
      padding: 6px 10px;
      cursor: pointer;
      font-size: 0.85rem;
    }
    .copy-btn:hover { background: rgba(230,237,243,0.14); }
    .copy-btn.ok { border-color: rgba(46,160,67,0.50); background: rgba(46,160,67,0.18); }
    .copy-btn.bad { border-color: rgba(209,36,47,0.50); background: rgba(209,36,47,0.18); }

    .grid { display: grid; grid-template-columns: 1fr; gap: 10px; }
    @media (min-width: 860px) { .grid { grid-template-columns: 1fr 1fr; } }

    ul { margin: 8px 0 8px 18px; }
    li { margin: 6px 0; }
    a { color: var(--accent); }
    .footer { margin-top: 26px; font-size: 0.9rem; color: var(--muted); }
  </style>
</head>
<body>
  <div class="container">
    <h1>Pi-hole v6: Nebula Sync (DHCP + Unbound) — Full Setup Notes <span class="badge">copy buttons included</span></h1>
    <p class="muted">This document is a cleaned, shareable version of the full chat. Passwords are intentionally <b>not</b> included. Replace placeholders where indicated.</p>

    <div class="card">
      <h2>Summary of your environment (as implemented)</h2>
      <ul>
        <li><b>Pi-hole #1 (PRIMARY):</b> 192.168.50.40 — DHCP enabled (DHCPv4 only)</li>
        <li><b>Pi-hole #2 (REPLICA):</b> 192.168.50.41 — DHCP disabled</li>
        <li><b>OS:</b> Debian 12 (bookworm) x86_64 on both</li>
        <li><b>Unbound:</b> installed on both; listening on <code>127.0.0.1#5053</code> (DNSSEC handled by Unbound)</li>
        <li><b>What must sync:</b> Local DNS A/AAAA records, regex rules, adlists</li>
        <li><b>What must NOT sync:</b> DHCP settings (and leases)</li>
        <li><b>Nebula host:</b> runs on 192.168.50.41</li>
        <li><b>Schedule:</b> daily at 03:33 America/New_York</li>
        <li><b>Gravity run after sync:</b> <b>disabled</b> (RUN_GRAVITY=false)</li>
      </ul>
      <div class="callout ok">
        <b>Key design rule:</b> Each Pi-hole uses only its <b>local Unbound</b> (<code>127.0.0.1#5053</code>) as upstream — no chaining Pi-holes.
      </div>
      <div class="callout">
        <b>Key safety rule:</b> Use <b>selective</b> sync (<code>FULL_SYNC=false</code>) so DHCP on 192.168.50.41 never turns on accidentally.
      </div>
    </div>

    <details open>
      <summary>1) Why Nebula Sync (and not gravity-sync / Orbital Sync)</summary>
      <div class="card" style="margin-top:10px;">
        <ul>
          <li><b>gravity-sync</b> was built for Pi-hole v5 file/database layout. Pi-hole v6 moved a lot of configuration into a TOML file and expanded the API.</li>
          <li><b>Orbital Sync</b> was archived (read-only) in 2025, so it is risky for a v6 setup that may change.</li>
          <li><b>Nebula Sync</b> is designed specifically for Pi-hole v6 and supports selective sync and scheduling.</li>
        </ul>
      </div>
    </details>

    <details open>
      <summary>2) Docker install on Debian 12 (Bookworm) — on 192.168.50.41</summary>
      <div class="card" style="margin-top:10px;">
        <p>These commands install Docker Engine + Compose plugin from Docker's official APT repository.</p>
        <pre><code>sudo apt-get update
sudo apt-get remove -y docker.io docker-compose docker-doc podman-docker containerd runc
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian $(. /etc/os-release && echo $VERSION_CODENAME) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
docker --version
docker compose version</code></pre>
        <div class="callout">
          Optional (so you can run docker without sudo): <code>sudo usermod -aG docker $USER</code> then log out/in.
        </div>
      </div>
    </details>

    <details open>
      <summary>3) Nebula Sync deployment (Docker Compose) — on 192.168.50.41</summary>
      <div class="card" style="margin-top:10px;">
        <p>Create a dedicated directory, a compose file, and an env file. Then start the container.</p>

        <h3>3.1 Create directory</h3>
        <pre><code>sudo mkdir -p /opt/nebula-sync</code></pre>

        <h3>3.2 Create /opt/nebula-sync/compose.yaml</h3>
        <pre><code>services:
  nebula-sync:
    image: ghcr.io/lovelaze/nebula-sync:latest
    container_name: nebula-sync
    restart: unless-stopped
    env_file: .env</code></pre>

        <h3>3.3 Create /opt/nebula-sync/.env (PASSWORDS REDACTED)</h3>
        <div class="callout bad"><b>Do not share admin passwords in Signal.</b> Use placeholders here and fill them locally.</div>
        <pre><code>PRIMARY=http://192.168.50.40|&lt;PIHOLE40_ADMIN_PASSWORD&gt;
REPLICAS=http://192.168.50.41|&lt;PIHOLE41_ADMIN_PASSWORD&gt;

# MUST be false due to DHCP asymmetry
FULL_SYNC=false

# Daily at 03:33 local time
CRON=33 3 * * *
TZ=America/New_York

# You chose: NO gravity runs after sync
RUN_GRAVITY=false

# Sync DNS config but only what you need
# - upstreams (local Unbound 127.0.0.1#5053)
# - hosts (Local DNS A/AAAA)
SYNC_CONFIG_DNS=true
SYNC_CONFIG_DNS_INCLUDE=upstreams,hosts

# Never sync DHCP (primary only)
SYNC_CONFIG_DHCP=false
SYNC_GRAVITY_DHCP_LEASES=false

# Keep other config sections off
SYNC_CONFIG_NTP=false
SYNC_CONFIG_RESOLVER=false
SYNC_CONFIG_DATABASE=false
SYNC_CONFIG_MISC=false
SYNC_CONFIG_DEBUG=false

# Sync adlists and domain/regex rules
SYNC_GRAVITY_AD_LIST=true
SYNC_GRAVITY_DOMAIN_LIST=true

# Not used in this setup
SYNC_GRAVITY_GROUP=false
SYNC_GRAVITY_CLIENT=false
SYNC_GRAVITY_AD_LIST_BY_GROUP=false
SYNC_GRAVITY_DOMAIN_LIST_BY_GROUP=false
SYNC_GRAVITY_CLIENT_BY_GROUP=false</code></pre>

        <h3>3.4 Start Nebula Sync</h3>
        <pre><code>cd /opt/nebula-sync
sudo docker compose up -d
sudo docker logs -f nebula-sync</code></pre>

        <div class="callout ok">
          Expected logs look like: <code>Running sync mode=selective</code> and <code>Sync completed</code>.
        </div>
      </div>
    </details>

    <details open>
      <summary>4) Permissions fix (if you see: cd /opt/nebula-sync: Permission denied)</summary>
      <div class="card" style="margin-top:10px;">
        <p>If the directory was locked down to root-only, change ownership so your user can access it, while keeping .env protected.</p>
        <pre><code>sudo chown -R dan:dan /opt/nebula-sync
sudo chmod 755 /opt/nebula-sync
sudo chmod 600 /opt/nebula-sync/.env
sudo chmod 644 /opt/nebula-sync/compose.yaml</code></pre>
      </div>
    </details>

    <details open>
      <summary>5) Pi-hole/Unbound constraints (why we configured it this way)</summary>
      <div class="card" style="margin-top:10px;">
        <div class="grid">
          <div class="card">
            <h3>5.1 DHCP rule (do not break this)</h3>
            <ul>
              <li>DHCP stays enabled only on 192.168.50.40.</li>
              <li>Therefore, Nebula must not sync DHCP config or leases.</li>
              <li>Therefore, FULL_SYNC must remain false.</li>
            </ul>
          </div>
          <div class="card">
            <h3>5.2 Unbound rule (redundancy without dependency)</h3>
            <ul>
              <li>Both Pi-holes use only <code>127.0.0.1#5053</code> for upstream.</li>
              <li>No upstream should point to the other Pi-hole's IP.</li>
              <li>This avoids a hidden dependency chain that ruins redundancy.</li>
            </ul>
          </div>
        </div>
        <h3>5.3 Optional: enforce upstreams via CLI on each Pi-hole</h3>
        <p>If you ever need to hard-set upstreams to local Unbound, use:</p>
        <pre><code>sudo pihole-FTL --config dns.upstreams '["127.0.0.1#5053"]'
sudo pihole-FTL --config dns.dnssec false</code></pre>
        <p class="muted">(DNSSEC is handled by Unbound in this design.)</p>
      </div>
    </details>

    <details open>
      <summary>6) Verification checklist (what to confirm after first sync)</summary>
      <div class="card" style="margin-top:10px;">
        <ol>
          <li><b>Replica DHCP is OFF</b> (192.168.50.41) and primary DHCP is ON (192.168.50.40).</li>
          <li><b>Both upstreams are ONLY</b> <code>127.0.0.1#5053</code>.</li>
          <li><b>Local DNS A/AAAA records</b> match between primary and replica.</li>
          <li><b>Adlists</b> match.</li>
          <li><b>Regex/domain rules</b> match.</li>
          <li><b>Nebula logs</b> show successful syncs at the scheduled time.</li>
        </ol>
      </div>
    </details>

    <details>
      <summary>7) Troubleshooting commands</summary>
      <div class="card" style="margin-top:10px;">
        <pre><code># Docker health
sudo systemctl status docker --no-pager

docker --version
docker compose version

# Nebula container status
cd /opt/nebula-sync
sudo docker compose ps
sudo docker logs --tail=200 nebula-sync

# Check runtime environment inside container (no secrets shown if you avoid printing PRIMARY/REPLICAS)
sudo docker exec nebula-sync env | egrep 'FULL_SYNC|CRON|TZ|RUN_GRAVITY|SYNC_' </code></pre>
      </div>
    </details>

    <details>
      <summary>8) How to send this via Signal (best practice)</summary>
      <div class="card" style="margin-top:10px;">
        <ol>
          <li>Attach this HTML file in Signal (it renders with copy buttons).</li>
          <li>Your dad opens it in a browser and can click <b>Copy</b> on any command block.</li>
          <li>Do <b>not</b> send passwords. Fill placeholders on your own systems.</li>
        </ol>
      </div>
    </details>

    <div class="footer">
      <p><b>Note:</b> This document intentionally omits sensitive values (passwords). If you need a version with secrets for your own use, keep it local and never send it over chat.</p>
    </div>

  </div>

  <script>
    // Adds a copy button to every PRE block.
    // Copies only the code text (not the button label).
    document.querySelectorAll('pre').forEach(pre => {
      const btn = document.createElement('button');
      btn.className = 'copy-btn';
      btn.type = 'button';
      btn.textContent = 'Copy';

      btn.addEventListener('click', async () => {
        const text = pre.innerText.replace(/\n?Copy\n?$/, '');
        try {
          await navigator.clipboard.writeText(text);
          btn.textContent = 'Copied';
          btn.classList.add('ok');
          setTimeout(() => {
            btn.textContent = 'Copy';
            btn.classList.remove('ok');
          }, 1200);
        } catch (e) {
          btn.textContent = 'Error';
          btn.classList.add('bad');
          setTimeout(() => {
            btn.textContent = 'Copy';
            btn.classList.remove('bad');
          }, 2000);
        }
      });

      pre.appendChild(btn);
    });
  </script>
</body>
</html>
