<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Estimate → WhatsApp — Mohammadi Printing</title>
  <style>
    body { font-family: Arial, sans-serif; max-width:800px; margin:20px auto; padding:10px; }
    input, textarea { width:100%; padding:8px; margin:6px 0; box-sizing:border-box; }
    .row { display:flex; gap:8px; }
    .row > input { flex:1 }
    table { width:100%; border-collapse:collapse; margin-top:8px; }
    th,td { border:1px solid #ddd; padding:6px; text-align:left; }
    button { padding:10px 14px; margin-top:10px; }
  </style>
</head>
<body>
  <h2>MOHAMMADIPRINTING PRESS - KHAMBHAT</h2>

  <label>Customer Name</label>
  <input id="custName" placeholder="Customer name">

  <label>Phone (with country code, e.g. 919825547625)</label>
  <input id="phone" placeholder="91xxxxxxxxxx">

  <label>Delivery / Process Days</label>
  <input id="delivery" placeholder="2/3">

  <h3>Items</h3>
  <table id="itemsTable">
    <thead><tr><th>Particulars</th><th>Qty</th><th>Rate</th><th>Amount</th><th></th></tr></thead>
    <tbody></tbody>
  </table>
  <button onclick="addRow()">Add item</button>

  <div style="margin-top:12px;">
    <label>Advance Paid (₹)</label>
    <input id="advance" value="0">
    <div style="margin-top:8px;">
      <strong>Total: ₹<span id="total">0</span></strong><br>
      <strong>Outstanding: ₹<span id="out">0</span></strong>
    </div>
  </div>

  <button onclick="openWhatsApp()">Open WhatsApp</button>

<script>
function addRow(part='', qty=1, rate=0){
  const tbody = document.querySelector('#itemsTable tbody');
  const tr = document.createElement('tr');
  tr.innerHTML = `
    <td><input class="part" value="${part}"></td>
    <td><input class="qty" type="number" value="${qty}" min="0"></td>
    <td><input class="rate" type="number" value="${rate}" min="0"></td>
    <td class="amt">0</td>
    <td><button onclick="this.closest('tr').remove(); recalc()">Delete</button></td>
  `;
  tbody.appendChild(tr);
  tbody.querySelectorAll('input.qty, input.rate').forEach(inp => inp.addEventListener('input', recalc));
  recalc();
}
function recalc(){
  const rows = document.querySelectorAll('#itemsTable tbody tr');
  let total=0;
  rows.forEach(r=>{
    const q = parseFloat(r.querySelector('.qty').value)||0;
    const rate = parseFloat(r.querySelector('.rate').value)||0;
    const amt = q*rate;
    r.querySelector('.amt').innerText = amt.toFixed(2);
    total += amt;
  });
  document.getElementById('total').innerText = total.toFixed(2);
  const adv = parseFloat(document.getElementById('advance').value)||0;
  document.getElementById('out').innerText = (total-adv).toFixed(2);
}
document.getElementById('advance').addEventListener('input', recalc);

// initial rows for convenience
addRow('1 BOOK', 150, 5); // example: change as you need
addRow('Extra cover', 50, 2);

function openWhatsApp(){
  const name = document.getElementById('custName').value || '';
  const phone = document.getElementById('phone').value.trim();
  if(!phone){ alert('Enter phone with country code'); return; }
  let msg = "MOHAMMADIPRINTING PRESS - KHAMBHAT\\n\\nESTIMATE\\n";
  msg += "Particulars:\\n";
  const rows = document.querySelectorAll('#itemsTable tbody tr');
  rows.forEach(r=>{
    const part = r.querySelector('.part').value;
    const q = r.querySelector('.qty').value;
    const rate = r.querySelector('.rate').value;
    const amt = (parseFloat(q||0)*parseFloat(rate||0)).toFixed(2);
    msg += `${part}  Qty: ${q}  Rate: ₹${rate}  Amt: ₹${amt}\\n`;
  });
  const total = document.getElementById('total').innerText;
  const advance = document.getElementById('advance').value || '0';
  const out = document.getElementById('out').innerText;
  const delivery = document.getElementById('delivery').value || '';
  msg += `\\nTotal: ₹${total}\\nAdvance Paid: ₹${advance}\\nOutstanding: ₹${out}\\n\\nDelivery Time: ${delivery}\\n\\nમોહંમદી પ્રિન્ટીંગ પ્રેસ\\nડાઃ સકીનાબેનના દવાખાના પાસે, વ્હોરવાડ, ખંભાત-388620\\nમો.9825547625`;
  const url = `https://wa.me/${phone}?text=${encodeURIComponent(msg)}`;
  window.open(url, '_blank');
}
</script>
</body>
</html>
