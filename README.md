# nmproject<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>User Authentication System — Demo</title>
  <meta name="description" content="Responsive, animated frontend demo for user authentication (signup / login / forgot password). Mock backend using localStorage. For production, replace with secure server-side logic." />
  <style>
    :root{
      --bg:#0f1724; /* deep navy */
      --card:#0b1220;
      --accent:#06b6d4; /* teal */
      --accent-2:#f97316; /* orange */
      --muted:#94a3b8;
      --glass: rgba(255,255,255,0.03);
      --success:#10b981;
      --danger:#ef4444;
      --max-width:960px;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      background: radial-gradient(1200px 600px at 10% 10%, rgba(6,182,212,0.08), transparent),
                  radial-gradient(1000px 500px at 90% 90%, rgba(249,115,22,0.06), transparent),
                  linear-gradient(180deg,var(--bg), #061020 80%);
      color:#e6eef6;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:40px 20px;
      gap:24px;
      min-height:100vh;
    }

    .wrap{
      width:100%;
      max-width:var(--max-width);
      display:grid;
      grid-template-columns: 1fr 420px;
      gap:28px;
      align-items:stretch;
      animation: popIn .6s ease both;
    }

    @media (max-width:900px){
      .wrap{grid-template-columns:1fr; padding:0 8px}
    }

    /* Left: marketing/info */
    .hero{
      background: linear-gradient(180deg, rgba(255,255,255,0.02), transparent);
      border-radius:18px;
      padding:36px;
      box-shadow: 0 8px 30px rgba(2,6,23,0.6);
      position:relative;
      overflow:hidden;
      display:flex;
      flex-direction:column;
      justify-content:center;
      gap:18px;
    }
    .brand{
      display:flex;align-items:center;gap:12px
    }
    .logo{
      width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent-2));
      display:flex;align-items:center;justify-content:center;font-weight:700;color:#04121a;font-size:20px;box-shadow:0 6px 18px rgba(6,182,212,0.12);
      transform:rotate(-8deg)
    }
    h1{margin:0;font-size:28px}
    p.lead{margin:0;color:var(--muted);line-height:1.45}

    .features{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin-top:8px}
    .feature{background:var(--glass);padding:12px;border-radius:10px;font-size:14px;color:var(--muted);display:flex;gap:10px;align-items:center}
    .feature .dot{width:10px;height:10px;border-radius:6px;background:linear-gradient(180deg,var(--accent),var(--accent-2));flex:0 0 10px}

    /* Right: form card */
    .card{
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:16px;padding:26px;box-shadow: 0 10px 40px rgba(2,6,23,0.6);
      min-height:420px;display:flex;flex-direction:column;justify-content:flex-start;gap:12px;backdrop-filter: blur(6px);
    }

    .tabs{display:flex;gap:6px;background:transparent;padding:4px;border-radius:12px}
    .tab{flex:1;padding:10px 12px;text-align:center;border-radius:10px;cursor:pointer;font-weight:600;user-select:none;transition:all .22s}
    .tab.active{background:linear-gradient(90deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));box-shadow:inset 0 -2px 0 rgba(255,255,255,0.02)}

    form{display:flex;flex-direction:column;gap:12px;margin-top:8px}
    label{font-size:13px;color:var(--muted)}
    input[type="text"],input[type="email"],input[type="password"]{width:100%;padding:12px 14px;border-radius:10px;background:transparent;border:1px solid rgba(255,255,255,0.05);color:inherit;outline:none;transition:box-shadow .18s,transform .12s}
    input:focus{box-shadow:0 6px 18px rgba(6,182,212,0.08);transform:translateY(-2px)}

    .row{display:flex;gap:12px}
    .row .col{flex:1}

    .actions{display:flex;align-items:center;justify-content:space-between;margin-top:6px}
    .btn{
      display:inline-flex;align-items:center;gap:8px;padding:10px 14px;border-radius:12px;border:0;background:linear-gradient(90deg,var(--accent),var(--accent-2));color:#04121a;font-weight:700;cursor:pointer;transition:transform .12s,box-shadow .12s
    }
    .btn:active{transform:translateY(1px)}

    .ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--muted);padding:8px 12px;border-radius:10px;cursor:pointer}

    .muted{color:var(--muted);font-size:13px}
    .link-like{color:var(--accent);cursor:pointer;font-weight:600}

    .notice{font-size:13px;padding:10px;border-radius:10px}
    .notice.success{background:linear-gradient(90deg, rgba(16,185,129,0.08), transparent);color:var(--success);border:1px solid rgba(16,185,129,0.08)}
    .notice.error{background:linear-gradient(90deg, rgba(239,68,68,0.06), transparent);color:var(--danger);border:1px solid rgba(239,68,68,0.06)}

    .pw-meter{height:8px;border-radius:8px;background:rgba(255,255,255,0.04);overflow:hidden}
    .pw-meter > i{display:block;height:100%;width:0%;background:linear-gradient(90deg,var(--danger),var(--accent));transition:width .35s ease}

    .small{font-size:12px;color:var(--muted)}

    footer.attribution{display:flex;justify-content:space-between;align-items:center;margin-top:auto;color:var(--muted);font-size:13px}

    /* subtle animated background shapes */
    .shape{position:absolute;border-radius:50%;filter:blur(60px);opacity:0.08}
    .shape.s1{width:260px;height:260px;background:linear-gradient(90deg,var(--accent),transparent);left:-80px;top:-40px}
    .shape.s2{width:180px;height:180px;background:linear-gradient(90deg,var(--accent-2),transparent);right:-60px;bottom:-40px}

    @keyframes popIn{from{opacity:0;transform:translateY(18px) scale(.995)}to{opacity:1;transform:none}}

    /* micro animations */
    .brand .logo{transition:transform .45s cubic-bezier(.2,.9,.3,1)}
    .brand:hover .logo{transform:rotate(0) scale(1.02)}

    .toast{position:fixed;right:20px;bottom:20px;min-width:220px;padding:12px;border-radius:10px;background:rgba(5,8,12,0.75);box-shadow:0 12px 30px rgba(2,6,23,0.6);backdrop-filter:blur(6px);color:#e6eef6}

    /* small screens tweaks */
    @media (max-width:420px){
      h1{font-size:22px}
      .logo{width:48px;height:48px}
      .card{padding:18px}
    }
  </style>
</head>
<body>
  <div class="wrap" role="main">
    <div class="hero" aria-hidden="false">
      <div class="shape s1"></div>
      <div class="shape s2"></div>
      <div class="brand">
        <div class="logo">UA</div>
        <div>
          <div style="font-size:13px;color:var(--muted);letter-spacing:0.6px">ACME Academy</div>
          <div style="font-weight:700">Secure Auth Suite</div>
        </div>
      </div>
      <h1>Modern authentication UI — secure & fast</h1>
      <p class="lead">A responsive, accessible frontend demo for signup, login, and password recovery flows. This template includes polished transitions, form validation, password strength meter, and a mock storage-based backend for local testing.</p>

      <div class="features" aria-hidden="true">
        <div class="feature"><div class="dot"></div><div><strong>Responsive</strong><div class="small">Looks great on mobile & desktop</div></div></div>
        <div class="feature"><div class="dot"></div><div><strong>Animated</strong><div class="small">Smooth micro-interactions</div></div></div>
        <div class="feature"><div class="dot"></div><div><strong>Accessible</strong><div class="small">Keyboard & screen reader friendly</div></div></div>
        <div class="feature"><div class="dot"></div><div><strong>Ready</strong><div class="small">Drop-in hooks for backend API</div></div></div>
      </div>

      <div style="margin-top:12px;display:flex;gap:10px;flex-wrap:wrap">
        <button class="btn" id="demoCreds">Use demo creds</button>
        <button class="ghost" id="downloadBtn" title="Save as .html">Download .html</button>
      </div>

      <div style="margin-top:12px;color:var(--muted);font-size:13px">Pro tip: this demo uses <strong>localStorage</strong> to simulate users — <span style="color:var(--accent)">not for production</span>.</div>
    </div>

    <aside class="card" aria-label="Authentication forms">
      <nav class="tabs" role="tablist" aria-label="Auth tabs">
        <div class="tab active" data-mode="login" role="tab" aria-selected="true">Login</div>
        <div class="tab" data-mode="signup" role="tab">Sign up</div>
        <div class="tab" data-mode="forgot" role="tab">Forgot</div>
      </nav>

      <div id="forms">
        <!-- LOGIN -->
        <form id="loginForm" data-mode="login" autocomplete="on">
          <label for="loginEmail">Email</label>
          <input id="loginEmail" name="email" type="email" inputmode="email" placeholder="you@domain.com" required />

          <label for="loginPassword">Password</label>
          <div style="position:relative">
            <input id="loginPassword" name="password" type="password" placeholder="••••••••" required />
            <button type="button" class="ghost" aria-label="Toggle password" id="toggleLoginPw" style="position:absolute;right:6px;top:8px">Show</button>
          </div>

          <div class="actions">
            <div class="muted">Don't have an account? <span class="link-like" id="toSignup">Create</span></div>
            <div style="display:flex;gap:8px;align-items:center">
              <button class="btn" type="submit">Login</button>
            </div>
          </div>
          <div style="margin-top:6px;display:flex;justify-content:space-between;align-items:center">
            <label style="display:flex;align-items:center;gap:8px"><input id="remember" type="checkbox" /> <span class="small">Remember me</span></label>
            <span class="link-like" id="toForgot">Forgot?</span>
          </div>
        </form>

        <!-- SIGNUP -->
        <form id="signupForm" data-mode="signup" style="display:none" autocomplete="on">
          <div class="row">
            <div class="col">
              <label for="firstName">First name</label>
              <input id="firstName" name="firstName" type="text" placeholder="First" required />
            </div>
            <div class="col">
              <label for="lastName">Last name</label>
              <input id="lastName" name="lastName" type="text" placeholder="Last" required />
            </div>
          </div>

          <label for="signupEmail">Email</label>
          <input id="signupEmail" name="email" type="email" placeholder="you@domain.com" required />

          <label for="signupPassword">Password</label>
          <div style="position:relative">
            <input id="signupPassword" name="password" type="password" placeholder="Create a strong password" required />
            <button type="button" class="ghost" aria-label="Toggle password" id="toggleSignupPw" style="position:absolute;right:6px;top:8px">Show</button>
          </div>
          <div class="pw-meter" aria-hidden="true"><i id="pwFill"></i></div>
          <div class="muted small">Password must be at least 8 characters, include letters and numbers.</div>

          <div style="display:flex;gap:8px;align-items:center;justify-content:space-between">
            <label style="display:flex;gap:8px;align-items:center"><input id="agreeTerms" type="checkbox" required /> <span class="small">I agree to terms</span></label>
            <button class="btn" type="submit">Create account</button>
          </div>

        </form>

        <!-- FORGOT -->
        <form id="forgotForm" data-mode="forgot" style="display:none" autocomplete="on">
          <label for="forgotEmail">Enter your email</label>
          <input id="forgotEmail" name="email" type="email" placeholder="you@domain.com" required />

          <div style="display:flex;gap:8px;align-items:center;justify-content:space-between">
            <div class="muted small">We will send a reset code (mock)</div>
            <button class="btn" type="submit">Send reset</button>
          </div>
        </form>

      </div>

      <div id="noticeWrap" style="margin-top:12px"></div>

      <footer class="attribution">
        <div class="small">Built for demo — frontend only</div>
        <div class="small">v1.0</div>
      </footer>
    </aside>
  </div>

  <div id="toast" class="toast" style="display:none"></div>

  <script>
    // --------------------------
    // Lightweight frontend auth demo
    // - Uses localStorage as a mocked user database (NOT secure)
    // - To integrate with a real backend, replace storage calls with fetch() to your API endpoints
    // --------------------------

    (function(){
      const $ = (sel, ctx=document) => ctx.querySelector(sel);
      const $$ = (sel, ctx=document) => Array.from(ctx.querySelectorAll(sel));

      const tabs = $$('.tab');
      const forms = {login:$('#loginForm'), signup:$('#signupForm'), forgot:$('#forgotForm')};
      const noticeWrap = $('#noticeWrap');
      const toast = $('#toast');

      function setActive(mode){
        tabs.forEach(t=>{t.classList.toggle('active', t.dataset.mode===mode); t.setAttribute('aria-selected', t.dataset.mode===mode)});
        Object.keys(forms).forEach(k=>{forms[k].style.display = (k===mode)?'block':'none'});
        // focus first input of active form
        const first = document.querySelector(`form[data-mode="${mode}"] input`);
        if(first) first.focus();
      }

      tabs.forEach(t=>t.addEventListener('click', ()=> setActive(t.dataset.mode)));

      // small helpers
      function showNotice(type, text){
        noticeWrap.innerHTML = `<div class="notice ${type}">${text}</div>`;
      }
      function showToast(txt, ttl=3500){
        toast.textContent = txt; toast.style.display='block'; toast.style.opacity=1;
        setTimeout(()=>{ toast.style.opacity=0; setTimeout(()=>toast.style.display='none',300)}, ttl);
      }

      // demo credentials
      const demo = {email:'demo@acme.test', password:'DemoPass123'};
      $('#demoCreds').addEventListener('click', ()=>{
        // ensure demo user exists
        saveUser({email:demo.email,password:demo.password,firstName:'Demo',lastName:'User'});
        $('#loginEmail').value = demo.email; $('#loginPassword').value=demo.password;
        showToast('Demo credentials filled');
      });

      // Download html button (save current page as .html)
      $('#downloadBtn').addEventListener('click', ()=>{
        const html = '<!doctype html>\n' + document.documentElement.outerHTML;
        const blob = new Blob([html], {type:'text/html'});
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a'); a.href = url; a.download = 'auth-demo.html'; document.body.appendChild(a); a.click(); a.remove();
        URL.revokeObjectURL(url);
      });

      // toggle pw visibility
      $('#toggleLoginPw').addEventListener('click', ()=>togglePw($('#loginPassword'), $('#toggleLoginPw')));
      $('#toggleSignupPw').addEventListener('click', ()=>togglePw($('#signupPassword'), $('#toggleSignupPw')));
      function togglePw(input, btn){
        if(input.type==='password'){ input.type='text'; btn.textContent='Hide'; } else { input.type='password'; btn.textContent='Show'; }
      }

      // password strength meter
      const pwFill = $('#pwFill');
      $('#signupPassword').addEventListener('input', (e)=>{
        const s = strength(e.target.value);
        pwFill.style.width = (s*33)+'%';
      });
      function strength(pw){
        let score=0; if(pw.length>=8) score++; if(/[A-Z]/.test(pw)) score++; if(/[0-9]/.test(pw)) score++; if(/[^A-Za-z0-9]/.test(pw)) score++;
        return Math.min(3, score);
      }

      // tabs linking
      $('#toSignup').addEventListener('click', ()=> setActive('signup'));
      $('#toForgot').addEventListener('click', ()=> setActive('forgot'));

      // mock "database" using localStorage
      const DB_KEY = 'auth_demo_users_v1';
      function getUsers(){ try { return JSON.parse(localStorage.getItem(DB_KEY) || '[]'); } catch(e){ return []; } }
      function saveUsers(arr){ localStorage.setItem(DB_KEY, JSON.stringify(arr)); }
      function findUser(email){ return getUsers().find(u=>u.email.toLowerCase()===email.toLowerCase()); }
      function saveUser({email,password,firstName='',lastName=''}){
        const users = getUsers().filter(u=>u.email.toLowerCase()!==email.toLowerCase());
        users.push({email,password,firstName,lastName,createdAt:new Date().toISOString()});
        saveUsers(users);
      }

      // signup handler
      $('#signupForm').addEventListener('submit', (ev)=>{
        ev.preventDefault();
        const email = $('#signupEmail').value.trim();
        const pw = $('#signupPassword').value;
        const first = $('#firstName').value.trim();
        const last = $('#lastName').value.trim();
        if(findUser(email)) { showNotice('error','An account with that email already exists.'); return; }
        if(pw.length<8){ showNotice('error','Password should be at least 8 characters.'); return; }
        // in production: hash password on server; do not store raw password
        saveUser({email,password:pw,firstName:first,lastName:last});
        showNotice('success','Account created — you can now login.');
        setActive('login');
      });

      // login handler
      $('#loginForm').addEventListener('submit', (ev)=>{
        ev.preventDefault();
        const email = $('#loginEmail').value.trim();
        const pw = $('#loginPassword').value;
        const user = findUser(email);
        if(!user){ showNotice('error','No account found for that email.'); return; }
        if(user.password !== pw){ showNotice('error','Incorrect password.'); return; }

        showNotice('success',`Welcome back, ${user.firstName || ''}!`);
        showToast('Login successful');
        // remember me demo
        if($('#remember').checked) localStorage.setItem('auth_demo_last', email);
        // attach token logic here for real app
      });

      // forgot handler (mock)
      $('#forgotForm').addEventListener('submit', (ev)=>{
        ev.preventDefault();
        const email = $('#forgotEmail').value.trim();
        const user = findUser(email);
        if(!user){ showNotice('error','No account found with that email.'); return; }
        // generate mock reset token and show
        const token = Math.random().toString(36).slice(2,10).toUpperCase();
        showNotice('success',`Reset code (mock): ${token} — use this to reset your password via your backend.`);
      });

      // pre-fill last remembered user
      const last = localStorage.getItem('auth_demo_last'); if(last){ $('#loginEmail').value = last; }

      // Accessibility: allow Enter to submit forms naturally, ensure focus outlines
      document.addEventListener('keyup', (e)=>{
        if(e.key==='Escape'){
          // clear notices
          noticeWrap.innerHTML='';
        }
      });

      // For development, seed one demo user
      (function seed(){ if(!findUser(demo.email)) saveUser({email:demo.email,password:demo.password,firstName:'Demo',lastName:'Account'}); })();

      // expose stub methods for integration (not necessary but helpful)
      window.authDemo = {
        getUsers, findUser, saveUser
      };

      // small safety: do not accidentally submit on navigation
      window.addEventListener('beforeunload', (e)=>{});

    })();

  </script>

</body>
</html>
