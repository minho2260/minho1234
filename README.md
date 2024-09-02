<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>월급 계산기</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .calculator {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 600px;
        }
        .calculator h2 {
            text-align: center;
            margin-bottom: 20px;
        }
        .calculator h3 {
            text-align: left;
            font-size: 1.25em;
            margin-bottom: 10px;
        }
        .calculator .input-group,
        .calculator .insurance-group {
            display: flex;
            align-items: center;
            margin-bottom: 5px;
            justify-content: space-between;
        }
        .calculator label {
            width: 25%;
        }
        .calculator input {
            width: 70%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            margin-right: 10px;
        }
        .calculator .wage-group {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 5px;
        }
        .calculator .wage-group label {
            width: 25%;
        }
        .calculator .wage-group input {
            width: 55%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            margin-right: 10px;
        }
        .calculator .min-wage-button {
            width: 20%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .calculator .min-wage-button:hover {
            background-color: #45a049;
        }
        .insurance-group-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        .insurance-group-container label {
            text-align: center;
            width: 100%;
        }
        .insurance-group {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .insurance-group button {
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            padding: 5px 10px;
            cursor: pointer;
            margin-top: 5px;
            width: 70px;
        }
        .insurance-group button.active {
            background-color: #B0B0B0; /* 버튼이 활성화되면 회색으로 변경 */
        }
        .insurance-group button:hover {
            background-color: #45a049;
        }
        .calculator .calculate-button {
            width: 100%;
            padding: 15px;
            margin-top: 20px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #4CAF50;
            color: white;
            font-size: 1.2em;
            cursor: pointer;
        }
        .calculator .calculate-button:hover {
            background-color: #45a049;
        }
        .result-container {
            display: flex;
            flex-direction: column;
            margin-top: 20px;
            font-size: 1.2em;
            font-weight: bold;
        }
        .result-item {
            margin-bottom: 10px;
        }
        .note {
            font-size: 0.8em;
            color: gray;
            margin-left: 5px;
        }
    </style>
</head>
<body>

<div class="calculator">
    <h2>월급 계산기</h2> <!-- 제목 변경 -->
    
    <h3>근로 조건</h3>
    
    <div class="wage-group">
        <label for="hourly-wage">시급:</label>
        <input type="text" id="hourly-wage" placeholder="시급을 입력하세요" oninput="formatNumberWithSuffix(this, '원')" onkeydown="handleBackspace(event, '원')">
        <button class="min-wage-button" onclick="setMinWage()">최저시급</button>
    </div>
    
    <div class="input-group">
        <label for="days-per-week">주당 근무일수:</label>
        <input type="text" id="days-per-week" placeholder="주당 근무일수를 입력하세요" oninput="formatNumberWithSuffix(this, '일')" onkeydown="handleBackspace(event, '일')">
    </div>
    
    <div class="input-group">
        <label for="hours-per-day">하루 근무시간:</label>
        <input type="text" id="hours-per-day" placeholder="하루 근무시간을 입력하세요" oninput="formatNumberWithSuffix(this, '시간')" onkeydown="handleBackspace(event, '시간')">
    </div>
    
    <div class="input-group">
        <label for="tax-rate">세율:</label>
        <input type="text" id="tax-rate" placeholder="세율을 입력하세요" step="0.1" oninput="formatNumberWithSuffix(this, '%')" onkeydown="handleBackspace(event, '%')">
    </div>
    
    <h3>소득세 및 4대 보험 선택</h3>
    
    <div class="insurance-group-container">
        <div class="insurance-group">
            <label>소득세 3.3%</label>
            <button id="income-tax-button" onclick="toggleInsuranceRate(3.3, this)">추가</button>
        </div>
        <div class="insurance-group">
            <label>국민연금</label>
            <button id="pension-button" onclick="toggleInsuranceRate(4.5, this)">추가</button>
        </div>
        <div class="insurance-group">
            <label>건강보험</label>
            <button id="health-button" onclick="toggleInsuranceRate(3.495, this)">추가</button>
        </div>
        <div class="insurance-group">
            <label>고용보험</label>
            <button id="employment-button" onclick="toggleInsuranceRate(0.9, this)">추가</button>
        </div>
        <div class="insurance-group">
            <label>산재보험</label>
            <button id="accident-button" onclick="toggleInsuranceRate(0.7, this)">추가</button>
        </div>
    </div>
    
    <button class="calculate-button" onclick="calculateTax()">세금 계산하기</button>
    
    <div class="result-container" id="result">
        <div class="result-item" id="weekly-income"></div>
        <div class="result-item" id="monthly-income"></div>
        <div class="result-item" id="tax-amount"></div>
        <div class="result-item" id="net-income"></div>
    </div>
</div>

<script>
    const minimumWage2024 = 9860; // 2024년 최저시급

    function setMinWage() {
        document.getElementById("hourly-wage").value = minimumWage2024.toLocaleString() + '원';
    }

    function formatNumberWithSuffix(input, suffix) {
        var value = input.value.replace(/[^0-9]/g, ''); // 숫자만 남기기
        var formattedValue = value.replace(/\B(?=(\d{3})+(?!\d))/g, ','); // 천단위 콤마 추가
        input.value = formattedValue + suffix;
    }

    function handleBackspace(event, suffix) {
        var input = event.target;
        var value = input.value.replace(suffix, '').replace(/,/g, ''); // 접미사와 콤마 제거

        if (event.key === "Backspace" && value.length > 0) {
            value = value.slice(0, -1); // 마지막 숫자 제거
            event.preventDefault();
            input.value = value.length > 0 ? parseInt(value).toLocaleString() + suffix : suffix;
        }

        // 커서가 접미사 앞에 오도록 설정
        setTimeout(function() {
            input.selectionStart = input.selectionEnd = input.value.length - suffix.length;
        }, 0);
    }

    function toggleInsuranceRate(rate, button) {
        var taxRateInput = document.getElementById("tax-rate");
        var currentRate = parseFloat(taxRateInput.value.replace(/[^0-9.]/g, '')) || 0; // 세율이 없으면 0으로 처리
        var newRate;

        if (button.classList.contains('active')) {
            newRate = (currentRate - rate).toFixed(3); // 이미 추가된 경우 제거
            button.classList.remove('active');
            button.style.backgroundColor = "#4CAF50"; // 원래 색상으로 복원
            button.textContent = '추가';
        } else {
            newRate = (currentRate + rate).toFixed(3); // 추가되지 않은 경우 추가
            button.classList.add('active');
            button.style.backgroundColor = "#B0B0B0"; // 회색으로 변경
            button.textContent = '취소';
        }

        taxRateInput.value = newRate > 0 ? newRate + '%' : ''; // 세율이 0 이하이면 빈 값으로 처리
    }

    function calculateTax() {
        var hourlyWage = parseFloat(document.getElementById("hourly-wage").value.replace(/[^0-9.]/g, ''));
        var daysPerWeek = parseFloat(document.getElementById("days-per-week").value.replace(/[^0-9.]/g, ''));
        var hoursPerDay = parseFloat(document.getElementById("hours-per-day").value.replace(/[^0-9.]/g, ''));
        var taxRate = parseFloat(document.getElementById("tax-rate").value.replace(/[^0-9.]/g, '')) || 0; // 세율이 없으면 0으로 처리

        if (isNaN(hourlyWage) || isNaN(daysPerWeek) || isNaN(hoursPerDay)) {
            alert("모든 필드를 올바르게 입력하세요.");
            return;
        }
        
        var totalIncomePerWeek = (hourlyWage * hoursPerDay * daysPerWeek).toFixed(0);
        var totalIncomePerMonth = (totalIncomePerWeek * 4).toFixed(0);
        
        var taxAmount = (totalIncomePerMonth * (taxRate / 100)).toFixed(0);
        var netIncome = (totalIncomePerMonth - taxAmount).toFixed(0);
        
        document.getElementById("weekly-income").innerText = "주 소득: " + parseInt(totalIncomePerWeek).toLocaleString() + "원";
        document.getElementById("monthly-income").innerHTML = "월 소득: " + parseInt(totalIncomePerMonth).toLocaleString() + "원 <span class='note'>(한 달을 4주로 가정)</span>";
        document.getElementById("tax-amount").innerText = "세금: " + parseInt(taxAmount).toLocaleString() + "원";
        document.getElementById("net-income").innerText = "실 수령액: " + parseInt(netIncome).toLocaleString() + "원";
    }
</script>

</body>
</html>
# -
