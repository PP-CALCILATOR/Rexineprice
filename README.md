<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>BizLite – Billing & Books</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #0f172a;
      color: #e5e7eb;
    }
    header {
      padding: 12px 16px;
      background: #020617;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 10;
    }
    header h1 { font-size: 18px; margin: 0; }
    header small { font-size: 11px; color: #9ca3af; }
    header button {
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid #475569;
      background: transparent;
      color: #e5e7eb;
      font-size: 12px;
      cursor: pointer;
    }
    main {
      max-width: 1100px;
      margin: 0 auto;
      padding: 16px;
    }
    .card {
      background: #020617;
      border-radius: 14px;
      padding: 16px;
      margin-bottom: 16px;
      border: 1px solid #1e293b;
    }
    h2 {
      margin: 0 0 12px;
      font-size: 16px;
    }
    label {
      display: block;
      font-size: 12px;
      margin-bottom: 4px;
      color: #9ca3af;
    }
    input, select, textarea {
      width: 100%;
      padding: 8px 10px;
      border-radius: 8px;
      border: 1px solid #334155;
      background: #020617;
      color: #e5e7eb;
      font-size: 13px;
      margin-bottom: 10px;
    }
    textarea { resize: vertical; min-height: 60px; }
    button.primary {
      padding: 8px 14px;
      border-radius: 999px;
      border: none;
      background: #22c55e;
      color: #022c22;
      font-weight: 600;
      font-size: 13px;
      cursor: pointer;
      margin-right: 6px;
      margin-top: 4px;
    }
    button.secondary {
      padding: 8px 14px;
      border-radius: 999px;
      border: 1px solid #4b5563;
      background: transparent;
      color: #e5e7eb;
      font-size: 13px;
      cursor: pointer;
      margin-right: 6px;
      margin-top: 4px;
    }
    .row {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
    }
    .col-4 { flex: 1 1 220px; }
    .col-6 { flex: 1 1 320px; }
    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 12px;
      margin-top: 8px;
    }
    th, td {
      border-bottom: 1px solid #1e293b;
      padding: 6px 4px;
      text-align: left;
    }
    th { color: #9ca3af; font-weight: 500; }
    .pill {
      display: inline-flex;
      align-items: center;
      padding: 3px 8px;
      border-radius: 999px;
      font-size: 11px;
    }
    .pill.ok { background: rgba(34,197,94,0.15); color: #4ade80; }
    .pill.pending { background: rgba(234,179,8,0.12); color: #facc15; }
    .pill.bad { background: rgba(248,113,113,0.12); color: #fca5a5; }
    .tabs {
      display: flex;
      gap: 8px;
      margin-bottom: 12px;
      flex-wrap: wrap;
    }
    .tab-btn {
      padding: 6px 12px;
      border-radius: 999px;
      border: 1px solid #4b5563;
      background: transparent;
      color: #e5e7eb;
      font-size: 12px;
      cursor: pointer;
    }
    .tab-btn.active {
      background: #e5e7eb;
      color: #020617;
      border-color: #e5e7eb;
    }
    .hidden { display: none !important; }
    .muted { color: #6b7280; font-size: 12px; }
    #authCard { max-width: 420px; margin: 40px auto; }
    @media (max-width: 640px) {
      header { flex-direction: column; align-items: flex-start; gap: 4px; }
    }
  </style>
</head>
<body>

<header>
  <div>
    <h1>BizLite</h1>
    <small>Minimal billing & books for your LLP</small>
  </div>
  <div>
    <span id="userEmail" class="muted"></span>
    <button id="logoutBtn" onclick="logout()" class="hidden">Logout</button>
  </div>
</header>

<main>
  <!-- AUTH -->
  <div id="authView">
    <div class="card" id="authCard">
      <h2 id="authTitle">Login</h2>
      <label>Email</label>
      <input id="authEmail" type="email" placeholder="you@example.com" />
      <label>Password</label>
      <input id="authPassword" type="password" placeholder="••••••••" />
      <button class="primary" onclick="handleAuth()">Continue</button>
      <button class="secondary" onclick="toggleMode()">Switch to Sign Up</button>
      <p class="muted" id="authError"></p>
    </div>
  </div>

  <!-- APP -->
  <div id="appView" class="hidden">

    <!-- NAV TABS -->
    <div class="tabs card">
      <button class="tab-btn active" data-tab="dash" onclick="switchTab('dash', this)">Dashboard</button>
      <button class="tab-btn" data-tab="customers" onclick="switchTab('customers', this)">Customers</button>
      <button class="tab-btn" data-tab="invoices" onclick="switchTab('invoices', this)">Invoices</button>
      <button class="tab-btn" data-tab="reports" onclick="switchTab('reports', this)">Reports</button>
    </div>

    <!-- DASHBOARD -->
    <section id="tab-dash" class="card">
      <h2>Snapshot</h2>
      <div class="row">
        <div class="col-4">
          <p class="muted">Total Invoiced (All time)</p>
          <h3 id="dashTotalInvoiced">₹0</h3>
        </div>
        <div class="col-4">
          <p class="muted">Pending Amount</p>
          <h3 id="dashPending">₹0</h3>
        </div>
        <div class="col-4">
          <p class="muted">Total Customers</p>
          <h3 id="dashCustomers">0</h3>
        </div>
      </div>
    </section>

    <!-- CUSTOMERS -->
    <section id="tab-customers" class="card hidden">
      <h2>Customers</h2>
      <div class="row">
        <div class="col-6">
          <h3 style="font-size:14px;">Add / Edit Customer</h3>
          <input type="hidden" id="custId" />
          <label>Name</label>
          <input id="custName" />
          <label>Phone</label>
          <input id="custPhone" />
          <label>GSTIN (optional)</label>
          <input id="custGST" />
          <label>Address</label>
          <textarea id="custAddress"></textarea>
          <button class="primary" onclick="saveCustomer()">Save Customer</button>
          <button class="secondary" onclick="resetCustomerForm()">Clear</button>
          <p class="muted" id="custMsg"></p>
        </div>
        <div class="col-6">
          <h3 style="font-size:14px;">Customer List</h3>
          <table>
            <thead>
              <tr>
                <th>Name</th>
                <th>Phone</th>
                <th>GST</th>
                <th></th>
              </tr>
            </thead>
            <tbody id="custTableBody"></tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- INVOICES -->
    <section id="tab-invoices" class="card hidden">
      <h2>Invoices</h2>
      <div class="row">
        <div class="col-6">
          <h3 style="font-size:14px;">Create Invoice</h3>
          <input type="hidden" id="invId" />
          <label>Customer</label>
          <select id="invCustomer"></select>
          <label>Invoice Number</label>
          <input id="invNumber" placeholder="INV-001" />
          <label>Date</label>
          <input id="invDate" type="date" />
          <label>Amount (₹)</label>
          <input id="invAmount" type="number" step="0.01" />
          <label>Status</label>
          <select id="invStatus">
            <option value="pending">Pending</option>
            <option value="paid">Paid</option>
            <option value="part">Partially Paid</option>
          </select>
          <label>Notes</label>
          <textarea id="invNotes" placeholder="Optional details"></textarea>
          <button class="primary" onclick="saveInvoice()">Save Invoice</button>
          <button class="secondary" onclick="resetInvoiceForm()">Clear</button>
          <p class="muted" id="invMsg"></p>
        </div>
        <div class="col-6">
          <h3 style="font-size:14px;">All Invoices</h3>
          <table>
            <thead>
              <tr>
                <th>No.</th>
                <th>Customer</th>
                <th>Date</th>
                <th>Amt</th>
                <th>Status</th>
                <th></th>
              </tr>
            </thead>
            <tbody id="invTableBody"></tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- REPORTS -->
    <section id="tab-reports" class="card hidden">
      <h2>Reports</h2>
      <div class="row">
        <!-- Report 1: All invoices by customer -->
        <div class="col-6">
          <h3 style="font-size:14px;">Invoices by Customer</h3>
          <select id="repCustomer"></select>
          <button class="secondary" onclick="runCustomerReport()">Run</button>
          <table>
            <thead>
              <tr>
                <th>No.</th>
                <th>Date</th>
                <th>Amt</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody id="repCustBody"></tbody>
          </table>
        </div>

        <!-- Report 2: Pending invoices -->
        <div class="col-6">
          <h3 style="font-size:14px;">Pending Invoices</h3>
          <table>
            <thead>
              <tr>
                <th>No.</th>
                <th>Customer</th>
                <th>Amt</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody id="repPendingBody"></tbody>
          </table>
        </div>
      </div>
    </section>

  </div>
</main>

<!-- Firebase SDKs -->
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore-compat.js"></script>

<script>
  // 1) YOUR FIREBASE CONFIG (REPLACE THIS BLOCK)
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "SENDER_ID",
    appId: "APP_ID"
  };

  firebase.initializeApp(firebaseConfig);
  const auth = firebase.auth();
  const db = firebase.firestore();

  // GLOBAL
  let authMode = "login"; // or "signup"
  let currentUser = null;
  let customers = [];
  let invoices = [];

  // AUTH UI
  function toggleMode() {
    authMode = authMode === "login" ? "signup" : "login";
    document.getElementById("authTitle").innerText =
      authMode === "login" ? "Login" : "Sign Up";
    document.querySelector('#authCard button.secondary').innerText =
      authMode === "login" ? "Switch to Sign Up" : "Switch to Login";
    document.getElementById("authError").innerText = "";
  }

  async function handleAuth() {
    const email = document.getElementById("authEmail").value.trim();
    const password = document.getElementById("authPassword").value.trim();
    const err = document.getElementById("authError");
    err.innerText = "";

    if (!email || !password) {
      err.innerText = "Email & password required.";
      return;
    }

    try {
      if (authMode === "login") {
        await auth.signInWithEmailAndPassword(email, password);
      } else {
        await auth.createUserWithEmailAndPassword(email, password);
      }
    } catch (e) {
      err.innerText = e.message;
    }
  }

  function logout() {
    auth.signOut();
  }

  auth.onAuthStateChanged(async (user) => {
    currentUser = user;
    if (user) {
      document.getElementById("authView").classList.add("hidden");
      document.getElementById("appView").classList.remove("hidden");
      document.getElementById("logoutBtn").classList.remove("hidden");
      document.getElementById("userEmail").innerText = user.email;
      await fetchData();
    } else {
      document.getElementById("authView").classList.remove("hidden");
      document.getElementById("appView").classList.add("hidden");
      document.getElementById("logoutBtn").classList.add("hidden");
      document.getElementById("userEmail").innerText = "";
    }
  });

  // TABS
  function switchTab(tabId, btn) {
    document.querySelectorAll(".tab-btn").forEach(b => b.classList.remove("active"));
    btn.classList.add("active");
    ["dash","customers","invoices","reports"].forEach(id => {
      document.getElementById("tab-" + id).classList.add("hidden");
    });
    document.getElementById("tab-" + tabId).classList.remove("hidden");
  }

  // DATA FETCH
  async function fetchData() {
    if (!currentUser) return;
    const uid = currentUser.uid;

    const custSnap = await db.collection("users").doc(uid).collection("customers").orderBy("name").get();
    customers = custSnap.docs.map(d => ({ id: d.id, ...d.data() }));

    const invSnap = await db.collection("users").doc(uid).collection("invoices").orderBy("date", "desc").get();
    invoices = invSnap.docs.map(d => ({ id: d.id, ...d.data() }));

    renderCustomers();
    renderCustomerOptions();
    renderInvoices();
    renderDashboard();
    renderReports();
  }

  // CUSTOMERS
  async function saveCustomer() {
    if (!currentUser) return;
    const uid = currentUser.uid;

    const id = document.getElementById("custId").value;
    const name = document.getElementById("custName").value.trim();
    const phone = document.getElementById("custPhone").value.trim();
    const gst = document.getElementById("custGST").value.trim();
    const address = document.getElementById("custAddress").value.trim();
    const msg = document.getElementById("custMsg");

    if (!name) {
      msg.innerText = "Name is required.";
      return;
    }

    try {
      const payload = { name, phone, gst, address, updatedAt: new Date() };
      const ref = db.collection("users").doc(uid).collection("customers");
      if (id) {
        await ref.doc(id).update(payload);
      } else {
        payload.createdAt = new Date();
        await ref.add(payload);
      }
      msg.innerText = "Saved.";
      setTimeout(() => msg.innerText = "", 1500);
      resetCustomerForm();
      await fetchData();
    } catch (e) {
      msg.innerText = e.message;
    }
  }

  function resetCustomerForm() {
    document.getElementById("custId").value = "";
    document.getElementById("custName").value = "";
    document.getElementById("custPhone").value = "";
    document.getElementById("custGST").value = "";
    document.getElementById("custAddress").value = "";
  }

  function editCustomer(id) {
    const c = customers.find(x => x.id === id);
    if (!c) return;
    document.getElementById("custId").value = c.id;
    document.getElementById("custName").value = c.name || "";
    document.getElementById("custPhone").value = c.phone || "";
    document.getElementById("custGST").value = c.gst || "";
    document.getElementById("custAddress").value = c.address || "";
  }

  async function deleteCustomer(id) {
    if (!currentUser) return;
    if (!confirm("Delete this customer?")) return;
    const uid = currentUser.uid;
    await db.collection("users").doc(uid).collection("customers").doc(id).delete();
    await fetchData();
  }

  function renderCustomers() {
    const body = document.getElementById("custTableBody");
    body.innerHTML = "";
    customers.forEach(c => {
      const tr = document.createElement("tr");
      tr.innerHTML = `
        <td>${c.name || ""}</td>
        <td>${c.phone || ""}</td>
        <td>${c.gst || ""}</td>
        <td>
          <button class="secondary" onclick="editCustomer('${c.id}')">Edit</button>
          <button class="secondary" onclick="deleteCustomer('${c.id}')">Del</button>
        </td>
      `;
      body.appendChild(tr);
    });
  }

  function renderCustomerOptions() {
    const select = document.getElementById("invCustomer");
    const repSelect = document.getElementById("repCustomer");
    select.innerHTML = `<option value="">Select</option>`;
    repSelect.innerHTML = `<option value="">Select</option>`;
    customers.forEach(c => {
      const opt = document.createElement("option");
      opt.value = c.id;
      opt.textContent = c.name;
      select.appendChild(opt);

      const opt2 = document.createElement("option");
      opt2.value = c.id;
      opt2.textContent = c.name;
      repSelect.appendChild(opt2);
    });
  }

  // INVOICES
  async function saveInvoice() {
    if (!currentUser) return;
    const uid = currentUser.uid;

    const id = document.getElementById("invId").value;
    const customerId = document.getElementById("invCustomer").value;
    const number = document.getElementById("invNumber").value.trim();
    const date = document.getElementById("invDate").value;
    const amount = parseFloat(document.getElementById("invAmount").value || "0");
    const status = document.getElementById("invStatus").value;
    const notes = document.getElementById("invNotes").value.trim();
    const msg = document.getElementById("invMsg");

    if (!customerId || !number || !date || !amount) {
      msg.innerText = "Customer, number, date, amount are required.";
      return;
    }

    try {
      const payload = {
        customerId,
        number,
        date,
        amount,
        status,
        notes,
        updatedAt: new Date()
      };
      const ref = db.collection("users").doc(uid).collection("invoices");
      if (id) {
        await ref.doc(id).update(payload);
      } else {
        payload.createdAt = new Date();
        await ref.add(payload);
      }
      msg.innerText = "Saved.";
      setTimeout(() => msg.innerText = "", 1500);
      resetInvoiceForm();
      await fetchData();
    } catch (e) {
      msg.innerText = e.message;
    }
  }

  function resetInvoiceForm() {
    document.getElementById("invId").value = "";
    document.getElementById("invCustomer").value = "";
    document.getElementById("invNumber").value = "";
    document.getElementById("invDate").value = "";
    document.getElementById("invAmount").value = "";
    document.getElementById("invStatus").value = "pending";
    document.getElementById("invNotes").value = "";
  }

  function editInvoice(id) {
    const inv = invoices.find(x => x.id === id);
    if (!inv) return;
    document.getElementById("invId").value = inv.id;
    document.getElementById("invCustomer").value = inv.customerId || "";
    document.getElementById("invNumber").value = inv.number || "";
    document.getElementById("invDate").value = inv.date || "";
    document.getElementById("invAmount").value = inv.amount || 0;
    document.getElementById("invStatus").value = inv.status || "pending";
    document.getElementById("invNotes").value = inv.notes || "";
  }

  async function deleteInvoice(id) {
    if (!currentUser) return;
    if (!confirm("Delete this invoice?")) return;
    const uid = currentUser.uid;
    await db.collection("users").doc(uid).collection("invoices").doc(id).delete();
    await fetchData();
  }

  function renderInvoices() {
    const body = document.getElementById("invTableBody");
    body.innerHTML = "";
    invoices.forEach(inv => {
      const c = customers.find(x => x.id === inv.customerId);
      const tr = document.createElement("tr");
      tr.innerHTML = `
        <td>${inv.number}</td>
        <td>${c ? c.name : "-"}</td>
        <td>${inv.date || ""}</td>
        <td>₹${inv.amount || 0}</td>
        <td>${renderStatusPill(inv.status)}</td>
        <td>
          <button class="secondary" onclick="editInvoice('${inv.id}')">Edit</button>
          <button class="secondary" onclick="deleteInvoice('${inv.id}')">Del</button>
        </td>
      `;
      body.appendChild(tr);
    });
  }

  function renderStatusPill(status) {
    if (status === "paid") return `<span class="pill ok">Paid</span>`;
    if (status === "part") return `<span class="pill pending">Part</span>`;
    return `<span class="pill bad">Pending</span>`;
  }

  // DASHBOARD & REPORTS
  function renderDashboard() {
    let total = 0;
    let pending = 0;
    invoices.forEach(inv => {
      total += inv.amount || 0;
      if     let sheetHeight = sheetHeightMeters * 39.3701;

    // number of pieces that actually fit
    let piecesAcross = Math.floor(sheetWidth / pieceW);
    let piecesDown = Math.floor(sheetHeight / pieceH);
    let totalPieces = piecesAcross * piecesDown;

    let costPerPiece = sheetCost / totalPieces;

    document.getElementById("result").innerHTML =
        "Total usable pieces: " + totalPieces +
        "<br>Cost per piece: ₹" + costPerPiece.toFixed(2);
}
</script>

</body>
</html>
