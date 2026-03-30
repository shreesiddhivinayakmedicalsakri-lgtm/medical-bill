<!DOCTYPE html>
<html>
<head>
<title>Shree Siddhivinayak Medical</title>

<style>
body {
  font-family: Arial;
  background: linear-gradient(to right, #36d1dc, #5b86e5);
  padding: 10px;
}

h2 {
  color: white;
}

.box {
  background: white;
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 15px;
}

.table-box {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  min-width: 500px;
}

td, th {
  border: 1px solid black;
  padding: 5px;
  text-align: center;
}

input, button {
  padding: 8px;
  margin: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  background: #5b86e5;
  color: white;
}
</style>
</head>

<body>

<h2>🏥 Shree Siddhivinayak Medical and General Store, Sakri</h2>

<div class="box">
<h3>Owner Login</h3>
<input id="pass" placeholder="Password">
<button onclick="login()">Login</button>
</div>

<div id="owner" class="box" style="display:none;">
<h3>Owner Panel</h3>

Bill No <input id="billno"><br>
Customer <input id="cust"><br>

<h4>Add Medicine</h4>
Name <input id="medname">
Batch <input id="batch">
Qty <input id="qty">
MRP <input id="mrp">

<button onclick="add()">Add</button>

<div class="table-box">
<table id="tbl">
<tr>
<th>Name</th>
<th>Batch</th>
<th>Qty</th>
<th>MRP</th>
<th>Total</th>
</tr>
</table>
</div>

<h3>Total ₹<span id="total">0</span></h3>
<button onclick="save()">Save Bill</button>
</div>

<div class="box">
<h3>Customer Panel</h3>
<input id="search" placeholder="Enter Bill No">
<button onclick="view()">Check Bill</button>

<div id="result"></div>
</div>

<script>
let total = 0;
let items = [];

function login(){
  if(pass.value=="1234"){
    owner.style.display="block";
  } else alert("Wrong Password");
}

function add(){
  let n = document.getElementById("medname").value;
  let b = batch.value;
  let q = Number(qty.value);
  let m = Number(mrp.value);

  let t = q * m;
  total += t;

  items.push({n,b,q,m,t});

  let row = tbl.insertRow();
  row.insertCell(0).innerText = n;
  row.insertCell(1).innerText = b;
  row.insertCell(2).innerText = q;
  row.insertCell(3).innerText = m;
  row.insertCell(4).innerText = t;

  document.getElementById("total").innerText = total;
}

function save(){
  let bill = {
    billno: billno.value,
    cust: cust.value,
    items: items,
    total: total
  };

  localStorage.setItem(bill.billno, JSON.stringify(bill));
  alert("Bill Saved!");
  location.reload();
}

function view(){
  let data = localStorage.getItem(search.value);

  if(!data){
    result.innerHTML = "❌ Bill Not Found";
    return;
  }

  let bill = JSON.parse(data);

  let html = "<h4>Customer: "+bill.cust+"</h4>";
  html += "<div class='table-box'><table><tr><th>Name</th><th>Batch</th><th>Qty</th><th>MRP</th><th>Total</th></tr>";

  bill.items.forEach(i=>{
    html += `<tr>
      <td>${i.n}</td>
      <td>${i.b}</td>
      <td>${i.q}</td>
      <td>${i.m}</td>
      <td>${i.t}</td>
    </tr>`;
  });

  html += "</table></div>";
  html += "<h3>Total ₹"+bill.total+"</h3>";

  result.innerHTML = html;
}
</script>

</body>
</html>
