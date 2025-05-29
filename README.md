<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>คำนวณรายได้ประจำวัน</title>
  <style>
    body {
      font-family: sans-serif;
      padding: 20px;
      background: #f2f2f2;
    }
    .container {
      background: white;
      padding: 20px;
      border-radius: 10px;
      max-width: 500px;
      margin: auto;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h2 {
      font-size: 24px;
      color: #003300;
    }
    h3 {
      margin-top: 20px;
      font-size: 20px;
      color: #004d00;
    }
    label {
      display: block;
      margin-top: 10px;
    }
    input, button, select {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      font-size: 16px;
    }
    .result {
      margin-top: 20px;
      background: #e6ffe6;
      padding: 15px;
      border-radius: 5px;
      color: #006600;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>คำนวณรายได้ประจำวัน</h2>

    <label>วันที่:</label>
    <input type="date" id="date" />

    <h3>รายรับ</h3>
    <label>รายได้จาก GRAB (บาท):</label>
    <input type="number" id="grab" />

    <label>รายได้จาก BOLT (บาท):</label>
    <input type="number" id="bolt" />

    <label>ทิป (บาท):</label>
    <input type="number" id="tip" />

    <label>รายรับอื่นๆ (บาท):</label>
    <input type="number" id="extraIncome" />

    <h3>รายจ่าย</h3>
    <label>ค่าน้ำมัน (บาท):</label>
    <input type="number" id="oil" />

    <label>รายจ่ายอื่นๆ (บาท):</label>
    <input type="number" id="otherExpense" />

    <h3>ระยะทางที่ใช้</h3>
    <label>เริ่มงาน (กม.):</label>
    <input type="number" id="startKm" />

    <label>เลิกงาน (กม.):</label>
    <input type="number" id="endKm" />

    <button onclick="calculate()">คำนวณ</button>

    <div class="result" id="result"></div>
  </div>

  <script>
    function calculate() {
      const date = document.getElementById('date').value;
      const grab = parseFloat(document.getElementById('grab').value) || 0;
      const bolt = parseFloat(document.getElementById('bolt').value) || 0;
      const tip = parseFloat(document.getElementById('tip').value) || 0;
      const extraIncome = parseFloat(document.getElementById('extraIncome').value) || 0;
      const oil = parseFloat(document.getElementById('oil').value) || 0;
      const otherExpense = parseFloat(document.getElementById('otherExpense').value) || 0;
      const startKm = parseFloat(document.getElementById('startKm').value) || 0;
      const endKm = parseFloat(document.getElementById('endKm').value) || 0;

      const distance = endKm - startKm;
      const boltCommission = bolt * 0.18;
      const boltAfter = bolt - boltCommission;

      const totalIncomeBeforeMaintenance = grab + boltAfter + extraIncome;
      const maintenance = totalIncomeBeforeMaintenance * 0.10;
      const totalExpenses = oil + otherExpense + maintenance + boltCommission;
      const netIncome = totalIncomeBeforeMaintenance - totalExpenses + tip;
      const costPerKm10 = distance > 0 ? (netIncome / (distance * 10)).toFixed(2) : "0.00";
      const halfIncomePlus10 = ((netIncome / 2) * 1.10).toFixed(2);

      const resultHTML = `
        <strong>สรุปผลประจำวันที่: ${date}</strong><br><br>
        <strong>รายรับ</strong><br>
        GRAB: ${grab.toFixed(2)} บาท<br>
        BOLT: ${bolt.toFixed(2)} บาท (หัก 18%: ${boltCommission.toFixed(2)} บาท เหลือ: ${boltAfter.toFixed(2)} บาท)<br>
        รายรับอื่นๆ: ${extraIncome.toFixed(2)} บาท<br>
        รวมรายรับก่อนหักค่าซ่อม: ${totalIncomeBeforeMaintenance.toFixed(2)} บาท<br><br>

        <strong>รายจ่าย</strong><br>
        ค่าซ่อม 10%: ${maintenance.toFixed(2)} บาท<br>
        ค่าน้ำมัน: ${oil.toFixed(2)} บาท<br>
        รายจ่ายอื่นๆ: ${otherExpense.toFixed(2)} บาท<br>
        ค่าคอมมิชชั่น BOLT: ${boltCommission.toFixed(2)} บาท<br>
        รวมรายจ่ายทั้งหมด: ${totalExpenses.toFixed(2)} บาท<br><br>

        <strong>ทิป:</strong> ${tip.toFixed(2)} บาท<br>
        <strong>รายได้สุทธิ:</strong> ${netIncome.toFixed(2)} บาท<br>
        <strong>หาร 2 +10%:</strong> ${halfIncomePlus10} บาท<br><br>

        <strong>ระยะทางที่ใช้:</strong> ${distance} กม.<br>
        <strong>บาทต่อกิโลเมตร (หาร 10):</strong> ${costPerKm10} บาท/กม.
      `;

      document.getElementById('result').innerHTML = resultHTML;
    }
  </script>
</body>
</html>
