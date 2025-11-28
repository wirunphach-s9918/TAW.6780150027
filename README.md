<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ระบบดูคะแนนสอบนักเรียน</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: #f3f4f6;
    }

    .app-container {
      background: #ffffff;
      width: 100%;
      max-width: 480px;
      padding: 24px 20px 28px;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(15, 23, 42, 0.15);
    }

    h1 {
      margin: 0 0 8px;
      font-size: 1.5rem;
      text-align: center;
      color: #111827;
    }

    .subtitle {
      text-align: center;
      font-size: 0.875rem;
      color: #6b7280;
      margin-bottom: 20px;
    }

    label {
      display: block;
      font-size: 0.875rem;
      margin-bottom: 6px;
      color: #374151;
    }

    input {
      width: 100%;
      padding: 10px 12px;
      border-radius: 10px;
      border: 1px solid #d1d5db;
      font-size: 0.95rem;
      outline: none;
      transition: border-color 0.15s ease, box-shadow 0.15s ease;
      background: #f9fafb;
    }

    input:focus {
      border-color: #2563eb;
      box-shadow: 0 0 0 1px rgba(37, 99, 235, 0.25);
      background: #ffffff;
    }

    .field-group {
      margin-bottom: 14px;
    }

    .password-wrapper {
      position: relative;
    }

    .toggle-password {
      position: absolute;
      top: 50%;
      right: 10px;
      transform: translateY(-50%);
      padding: 4px 8px;
      font-size: 0.75rem;
      border-radius: 999px;
      border: none;
      background: #e5e7eb;
      cursor: pointer;
      color: #374151;
    }

    .btn {
      width: 100%;
      margin-top: 6px;
      padding: 10px 12px;
      border-radius: 999px;
      border: none;
      font-size: 0.98rem;
      font-weight: 600;
      cursor: pointer;
      background: #2563eb;
      color: white;
      transition: background 0.15s ease, transform 0.05s ease;
    }

    .btn:hover {
      background: #1d4ed8;
    }

    .btn:active {
      transform: translateY(1px);
    }

    .error-message {
      font-size: 0.8rem;
      color: #b91c1c;
      margin-top: 6px;
      min-height: 1em;
    }

    .success-message {
      font-size: 0.85rem;
      color: #047857;
      margin-top: 8px;
    }

    .divider {
      height: 1px;
      background: #e5e7eb;
      margin: 18px 0 14px;
    }

    .dashboard {
      display: none;
      font-size: 0.9rem;
    }

    .student-info {
      margin-bottom: 10px;
      background: #f9fafb;
      border-radius: 12px;
      padding: 10px 12px;
      border: 1px solid #e5e7eb;
    }

    .student-info div {
      margin-bottom: 2px;
      color: #111827;
    }

    .student-info span.label {
      font-weight: 600;
      color: #4b5563;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.8rem;
      margin-top: 8px;
      border-radius: 12px;
      overflow: hidden;
    }

    thead {
      background: #eff6ff;
    }

    th, td {
      padding: 8px 6px;
      text-align: left;
      border-bottom: 1px solid #e5e7eb;
    }

    th {
      font-weight: 600;
      color: #1f2937;
    }

    tbody tr:nth-child(even) {
      background: #f9fafb;
    }

    .score-badge {
      display: inline-block;
      min-width: 32px;
      text-align: center;
      padding: 2px 6px;
      border-radius: 999px;
      font-weight: 600;
      background: #e0f2fe;
      color: #075985;
    }

    .footer {
      margin-top: 14px;
      font-size: 0.72rem;
      color: #9ca3af;
      text-align: center;
    }

    .logout-btn {
      margin-top: 10px;
      padding: 7px 12px;
      border-radius: 999px;
      border: 1px solid #d1d5db;
      background: #f9fafb;
      font-size: 0.8rem;
      cursor: pointer;
      color: #374151;
    }

    .logout-btn:hover {
      background: #f3f4f6;
    }

    @media (max-width: 480px) {
      .app-container {
        border-radius: 0;
        min-height: 100vh;
        max-width: 100%;
        box-shadow: none;
      }
    }
  </style>
</head>
<body>
  <div class="app-container">
    <h1>ระบบดูคะแนนสอบ</h1>
    <div class="subtitle">เข้าสู่ระบบด้วยรหัสนักเรียนและเลขบัตรประชาชน</div>

    <!-- Login Form -->
    <form id="loginForm">
      <div class="field-group">
        <label for="studentId">รหัสประจำตัวนักเรียน</label>
        <input
          type="text"
          id="studentId"
          placeholder="เช่น 65001"
          autocomplete="username"
          required
        />
      </div>

      <div class="field-group">
        <label for="nationalId">เลขบัตรประชาชน (13 หลัก)</label>
        <div class="password-wrapper">
          <input
            type="password"
            id="nationalId"
            placeholder="เช่น 1234567890123"
            autocomplete="current-password"
            required
          />
          <button type="button" class="toggle-password" id="togglePassword">
            แสดง
          </button>
        </div>
      </div>

      <button type="submit" class="btn">เข้าสู่ระบบ</button>
      <div class="error-message" id="errorMessage"></div>
    </form>

    <div class="divider"></div>

    <!-- Dashboard -->
    <div class="dashboard" id="dashboard">
      <div class="student-info" id="studentInfo"></div>

      <div><strong>ตารางคะแนนสอบ</strong></div>
      <table>
        <thead>
          <tr>
            <th>วิชา</th>
            <th>คะแนน</th>
            <th>วันที่สอบ</th>
            <th>ประเภท</th>
          </tr>
        </thead>
        <tbody id="scoreTableBody"></tbody>
      </table>

      <button class="logout-btn" id="logoutBtn">ออกจากระบบ</button>
      <div class="success-message">เข้าสู่ระบบสำเร็จ แสดงผลคะแนนด้านบน</div>
    </div>

    <div class="footer">
      ตัวอย่างระบบสำหรับการเรียนรู้ (ไม่ใช้จริงในงานราชการ)<br />
      *ไม่ควรใช้เลขบัตรประชาชนจริงในระบบที่ไม่ปลอดภัย*
    </div>
  </div>

  <script>
    // ข้อมูลตัวอย่าง จำลองฐานข้อมูลนักเรียน
    const studentsDB = {
      "65001": {
        nationalId: "1234567890123",
        name: "เด็กชายตัวอย่าง หนึ่ง",
        classroom: "ป.5/5",
        scores: [
          { subject: "คณิตศาสตร์", score: 85, date: "2025-10-15", type: "กลางภาค" },
          { subject: "ภาษาอังกฤษ", score: 90, date: "2025-10-20", type: "กลางภาค" },
          { subject: "วิทยาศาสตร์", score: 78, date: "2025-10-25", type: "เก็บคะแนน" }
        ]
      },
      "65002": {
        nationalId: "9876543210123",
        name: "เด็กหญิงตัวอย่าง สอง",
        classroom: "ป.5/5",
        scores: [
          { subject: "คณิตศาสตร์", score: 92, date: "2025-10-15", type: "กลางภาค" },
          { subject: "ภาษาไทย", score: 88, date: "2025-10-18", type: "กลางภาค" },
          { subject: "วิทยาศาสตร์", score: 81, date: "2025-10-25", type: "เก็บคะแนน" }
        ]
      }
    };

    const loginForm = document.getElementById("loginForm");
    const studentIdInput = document.getElementById("studentId");
    const nationalIdInput = document.getElementById("nationalId");
    const errorMessageEl = document.getElementById("errorMessage");
    const dashboardEl = document.getElementById("dashboard");
    const studentInfoEl = document.getElementById("studentInfo");
    const scoreTableBodyEl = document.getElementById("scoreTableBody");
    const togglePasswordBtn = document.getElementById("togglePassword");
    const logoutBtn = document.getElementById("logoutBtn");

    // แสดง/ซ่อนรหัสผ่าน
    togglePasswordBtn.addEventListener("click", () => {
      const currentType = nationalIdInput.getAttribute("type");
      const isPassword = currentType === "password";
      nationalIdInput.setAttribute("type", isPassword ? "text" : "password");
      togglePasswordBtn.textContent = isPassword ? "ซ่อน" : "แสดง";
    });

    // ฟังก์ชันแสดงข้อมูลนักเรียนและคะแนน
    function renderStudentDashboard(studentId, studentData) {
      // ข้อมูลนักเรียน
      studentInfoEl.innerHTML = `
        <div><span class="label">รหัสนักเรียน:</span> ${studentId}</div>
        <div><span class="label">ชื่อ-นามสกุล:</span> ${studentData.name}</div>
        <div><span class="label">ชั้นเรียน:</span> ${studentData.classroom}</div>
      `;

      // ตารางคะแนน
      scoreTableBodyEl.innerHTML = "";
      studentData.scores.forEach((item) => {
        const tr = document.createElement("tr");
        tr.innerHTML = `
          <td>${item.subject}</td>
          <td><span class="score-badge">${item.score}</span></td>
          <td>${item.date}</td>
          <td>${item.type}</td>
        `;
        scoreTableBodyEl.appendChild(tr);
      });

      dashboardEl.style.display = "block";
    }

    // ฟังก์ชัน reset
    function resetApp() {
      loginForm.reset();
      dashboardEl.style.display = "none";
      errorMessageEl.textContent = "";
      nationalIdInput.setAttribute("type", "password");
      togglePasswordBtn.textContent = "แสดง";
    }

    // กดออกจากระบบ
    logoutBtn.addEventListener("click", () => {
      resetApp();
    });

    // การเข้าสู่ระบบ
    loginForm.addEventListener("submit", (event) => {
      event.preventDefault();
      const studentId = studentIdInput.value.trim();
      const nationalId = nationalIdInput.value.trim();

      // ตรวจสอบเบื้องต้น
      if (!studentId || !nationalId) {
        errorMessageEl.textContent = "กรุณากรอกข้อมูลให้ครบถ้วน";
        dashboardEl.style.display = "none";
        return;
      }

      if (nationalId.length !== 13 || !/^[0-9]+$/.test(nationalId)) {
        errorMessageEl.textContent = "เลขบัตรประชาชนต้องเป็นตัวเลข 13 หลัก";
        dashboardEl.style.display = "none";
        return;
      }

      const studentData = studentsDB[studentId];

      if (!studentData || studentData.nationalId !== nationalId) {
        errorMessageEl.textContent = "รหัสนักเรียนหรือเลขบัตรประชาชนไม่ถูกต้อง";
        dashboardEl.style.display = "none";
        return;
      }

      // ถ้าข้อมูลถูกต้อง
      errorMessageEl.textContent = "";
      renderStudentDashboard(studentId, studentData);
    });
  </script>
</body>
</html>
