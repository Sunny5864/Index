
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Grow Together Fund - Dashboard</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
  <style>
    body {
      background: #f4f6f9;
      font-family: 'Segoe UI', sans-serif;
    }
    .summary-card {
      border-radius: 1rem;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      padding: 1.5rem;
    }
  </style>
</head>
<body>
  <div class="container my-4">
    <h2 class="text-center mb-4">📊Grow Together Fund Dashboard</h2>

    <div class="row text-center">
      <div class="col-md-4">
        <div class="bg-success text-white summary-card">
          <h5>Total Deposits</h5>
          <h3 id="totalDeposit">৳0</h3>
        </div>
      </div>
      <div class="col-md-4">
        <div class="bg-danger text-white summary-card">
          <h5>Total Loans</h5>
          <h3 id="totalLoan">৳0</h3>
        </div>
      </div>
      <div class="col-md-4">
        <div class="bg-warning text-dark summary-card">
          <h5>Total Investment</h5>
          <h3 id="totalInvest">৳0</h3>
        </div>
      </div>
    </div>
<div class="row text-center mt-4">
  <div class="col-md-6 offset-md-3">
    <div class="bg-primary text-white summary-card">
      <h5>Balance</h5>
      <h3 id="balance">৳0</h3>
    </div>
  </div>
</div>



    <div class="mt-5">
      <h4>📅 Monthly Summary</h4>
      <input type="month" id="monthPicker" class="form-control w-25 mb-3" onchange="filterByMonth()" />
      <ul id="summaryList" class="list-group"></ul>
    </div>
  </div>

  <script>
    const data = JSON.parse(localStorage.getItem("hisabData") || "[]");

    const totalDeposit = data.filter(x => x.type === "deposit").reduce((sum, e) => sum + e.amount, 0);
    const totalLoan = data.filter(x => x.type === "loan").reduce((sum, e) => sum + e.amount, 0);
    const totalInvest = data.filter(x => x.type === "invest").reduce((sum, e) => sum + e.amount, 0);
const balance = totalDeposit - (totalLoan + totalInvest);
   
 document.getElementById("totalDeposit").innerText = `৳${totalDeposit}`;
    document.getElementById("totalLoan").innerText = `৳${totalLoan}`;
    document.getElementById("totalInvest").innerText = `৳${totalInvest}`;
document.getElementById("balance").innerText = `৳${balance}`;

    function filterByMonth() {
      const month = document.getElementById("monthPicker").value;
      const ul = document.getElementById("summaryList");
      ul.innerHTML = "";
      if (!month) return;

      const filtered = data.filter(e => e.date.startsWith(month));
      const grouped = {};

      filtered.forEach(e => {
        if (!grouped[e.name]) grouped[e.name] = { deposit: 0, loan: 0, invest: 0 };
        grouped[e.name][e.type] += e.amount;
      });

      for (let name in grouped) {
        const li = document.createElement("li");
        li.className = "list-group-item";
        li.innerHTML = `<b>${name}</b> — Deposit: ৳${grouped[name].deposit}, Loan: ৳${grouped[name].loan}, Invest: ৳${grouped[name].invest}`;
        ul.appendChild(li);
      }
    }
  </script>
</body>
</html>
