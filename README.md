<!-- هذا الملف يحتوي على المشروع كامل -->

1. index.html
2. style.css
3. script.js

---

// index.html
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <title>بوابة الرسوم الشاملة السعودية</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>حاسبة الرسوم الشاملة السعودية</h1>

        <label>تاريخ انتهاء الإقامة:</label>
        <input type="date" id="iqamaEnd">

        <label>نوع العمالة:</label>
        <select id="workerType">
            <option value="institution">عمالة مؤسسات</option>
            <option value="household">عمالة منزلية</option>
        </select>

        <label>هل التطويف أول مرة؟</label>
        <select id="taweef">
            <option value="first">أول مرة</option>
            <option value="second">تاني مرة</option>
            <option value="none">لا يوجد</option>
        </select>

        <label>عدد العمال الكلي:</label>
        <input type="number" id="totalWorkers" value="1">

        <label>هل المؤسسة بها موظف سعودي؟</label>
        <select id="saudiEmployee">
            <option value="yes">نعم</option>
            <option value="no">لا</option>
        </select>

        <button onclick="calculateFees()">احسب الرسوم</button>

        <h2>النتيجة:</h2>
        <div id="result"></div>
    </div>

    <script src="script.js"></script>
</body>
</html>

---

// style.css
body {
    font-family: Arial, sans-serif;
    direction: rtl;
    text-align: right;
    background-color: #f5f5f5;
    padding: 20px;
}

.container {
    max-width: 600px;
    margin: auto;
    background: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px #ccc;
}

label, select, input {
    display: block;
    width: 100%;
    margin: 10px 0;
}

button {
    background-color: #0066cc;
    color: #fff;
    padding: 10px;
    width: 100%;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #004d99;
}

#result {
    background-color: #e8f4ff;
    padding: 10px;
    border-radius: 5px;
    margin-top: 10px;
}

---

// script.js
function calculateFees() {
    const iqamaEnd = new Date(document.getElementById('iqamaEnd').value);
    const workerType = document.getElementById('workerType').value;
    const taweef = document.getElementById('taweef').value;
    const totalWorkers = parseInt(document.getElementById('totalWorkers').value);
    const hasSaudi = document.getElementById('saudiEmployee').value === "yes";

    const today = new Date();
    let resultText = "";

    // حساب رسوم الإقامة
    const iqamaEndTime = iqamaEnd.getTime();
    const todayTime = today.getTime();
    const oneDay = 1000 * 60 * 60 * 24;
    const daysLate = Math.max(Math.floor((todayTime - iqamaEndTime) / oneDay), 0);

    let iqamaFee = 0;
    if (workerType === "household") {
        iqamaFee = 600;
    } else {
        iqamaFee = 1000;
    }

    // الغرامة على التطويف
    let taweefFee = 0;
    if (taweef === "first") taweefFee = 500;
    else if (taweef === "second") taweefFee = 1000;

    // رسوم كرت العمل
    let workCardFee = 0;
    const iqamaYear = iqamaEnd.getFullYear();

    if (workerType === "institution") {
        let exemptWorkers = 0;
        if (totalWorkers <= 9) {
            exemptWorkers = hasSaudi ? 4 : 2;
        }
        const nonExemptWorkers = Math.max(totalWorkers - exemptWorkers, 0);

        if (iqamaYear <= 2026) {
            workCardFee = nonExemptWorkers * 100;
        } else {
            workCardFee = nonExemptWorkers * 9600;
        }
    } else {
        workCardFee = 100;
    }

    const totalFee = iqamaFee + taweefFee + workCardFee;

    resultText += `رسوم الإقامة: ${iqamaFee} ريال <br>`;
    resultText += `غرامة التطويف: ${taweefFee} ريال <br>`;
    resultText += `رسوم كرت العمل: ${workCardFee} ريال <br>`;
    resultText += `<b>المجموع الكلي: ${totalFee} ريال</b>`;

    document.getElementById('result').innerHTML = resultText;
}
