<!DOCTYPE html><html lang="th">
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
    input, button, select {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      font-size: 16px;
    }
    .result {
      margin-top: 20px;
      background: #e6ffe6;
      padding: 15px;
      border-radius: 5px;
      color: #006600;
    }
    h2 {
      font-size: 24px;
      font-weight: bold;
      color: #333;
    }
    h3 {
      font-size: 20px;
      font-weight: bold;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>คำนวณรายได้ประจำวัน</h2><label>วันที่:</label>
<input type="date" id="date" />

<h3>รายรับ</h3>
<label>รายได้จาก GRAB (บาท):</label>
<input type="number" id="grab" />

<label>รายได้จาก BOLT (บาท):</label>
<input type="number" id="bolt" />

<label>ทิป (บาท):</label>
<input type="number" id="tip" />

<label>อื่นๆ (บาท):</label>
<input type="number" id="otherIncome" />

<h3>รายจ่าย</h3>
<label>ค่าน้ำมัน:</label>
<input type="number" id="electricity" />

<label>รายจ่ายอื่นๆ:</label>
<input type="number" id="otherExpense" />

<h3>ระยะทางที่ใช้</h3>
<label>เริ่มงาน (กม.):</label>
<input type="number" id="startDistance" />

<label>เลิกงาน (กม.):</label>
<input type="number" id="endDistance" />

<button onclick="calculate()">คำนวณ</button>

<div class="result" id="result"></div>

  </div>  <script>
    function calculate() {
      const date = document.getElementById('date').value;
      const grab = parseFloat(document.getElementById('grab').value) || 0;
      const boltOriginal = parseFloat(document.getElementById('bolt').value) || 0;
      const electricity = parseFloat(document.getElementById('electricity').value) || 0;
      const other = parseFloat(document.getElementById('otherExpense').value) || 0;
      const tip = parseFloat(document.getElementById('tip').value) || 0;
      const otherIncome = parseFloat(document.getElementById('otherIncome').value) || 0;
      const startDistance = parseFloat(document.getElementById('startDistance').value) || 0;
      const endDistance = parseFloat(document.getElementById('endDistance').value) || 0;

      const distance = endDistance - startDistance;
      const boltCommission = boltOriginal * 0.18;
      const boltAfterCommission = boltOriginal - boltCommission;
      const totalIncome = grab + boltAfterCommission + otherIncome;
      const maintenance = totalIncome * 0.10;
      const totalExpense = maintenance + electricity + other + boltCommission;
      const netIncome = totalIncome - totalExpense + tip;
      const incomePerKm = distance > 0 ? (netIncome / (distance * 10)).toFixed(2) : 0;
      const halfIncomePlus10Percent = ((netIncome / 2) * 1.10).toFixed(2);

      const resultHTML = `
        <strong>สรุปผลวันที่: ${date}</strong><br><br>
        รายได้ GRAB: ${grab.toFixed(2)} บาท<br>
        รายได้ BOLT (หักแล้ว 18%): ${boltAfterCommission.toFixed(2)} บาท (หักไป: ${boltCommission.toFixed(2)} บาท)<br>
        รายได้อื่นๆ: ${otherIncome.toFixed(2)} บาท<br>
        รวมรายได้: ${totalIncome.toFixed(2)} บาท<br>
        ค่าซ่อม 10%: ${maintenance.toFixed(2)} บาท<br>
        ค่าน้ำมัน: ${electricity.toFixed(2)} บาท<br>
        รายจ่ายอื่น ๆ: ${other.toFixed(2)} บาท<br>
        ค่าคอมมิชชั่น BOLT: ${boltCommission.toFixed(2)} บาท<br>
        ทิป: ${tip.toFixed(2)} บาท<br>
        <strong>รายได้สุทธิ: ${netIncome.toFixed(2)} บาท</strong><br>
        รายได้หาร 2 +10%: ${halfIncomePlus10Percent} บาท<br><br>
        ระยะทางที่ใช้: ${distance} กม.<br>
        บาทต่อกิโลเมตร (หาร 10): ${incomePerKm} บาท/กม.
      `;

      document.getElementById('result').innerHTML = resultHTML;
    }
  </script></body>
</html>
