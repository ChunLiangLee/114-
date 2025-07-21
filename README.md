<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>114學年度新聞系幹部點名系統</title>
  <style>
    body { font-family: "微軟正黑體", sans-serif; max-width: 480px; margin: 2rem auto; padding: 1rem; }
    h1 { text-align: center; color: #333; }
    label { display: block; margin: 0.5rem 0 0.2rem; }
    input { width: 100%; padding: 0.5rem; font-size: 1rem; }
    button { margin-top: 1rem; padding: 0.7rem; width: 100%; font-size: 1rem; background: #2d8cf0; color: white; border: none; cursor: pointer; }
    .message { margin-top: 1rem; padding: 0.8rem; border-radius: 4px; font-size: 1rem; }
    .success { background: #e0f7ea; color: #2e7d32; }
    .duplicate { background: #fff3cd; color: #856404; }
  </style>
</head>
<body>
  <h1>📋 114學年度新聞系幹部點名系統</h1>
  <form id="attendanceForm">
    <label>姓名（必填）<input type="text" name="name" required /></label>
    <label>學號（必填）<input type="text" name="id" required /></label>
    <label>系級（必填）<input type="text" name="dept" required /></label>
    <label>Email（選填）<input type="email" name="email" /></label>
    <button type="submit">✅ 送出點名</button>
  </form>
  <div id="msg" class="message" style="display:none;"></div>

  <script>
    document.getElementById('attendanceForm').addEventListener('submit', async e => {
      e.preventDefault();
      const form = e.target;
      const data = {
        name: form.name.value.trim(),
        id: form.id.value.trim(),
        dept: form.dept.value.trim(),
        email: form.email.value.trim(),
      };
      const msgEl = document.getElementById('msg');
      msgEl.style.display = 'none';

      try {
        const res = await fetch('https://script.google.com/macros/s/AKfycbwkZDcBES6vR99B2sfw4L-OL4D0dwtMp2k-C_PgtXx5G_VQ/exec', {
          method: 'POST',
          contentType: 'application/json',
          body: JSON.stringify(data)
        });
        const result = await res.json();

        msgEl.style.display = 'block';
        if (result.status === 'success') {
          msgEl.textContent = '✅ 您已成功點名！';
          msgEl.className = 'message success';
          form.reset();
        } else {
          msgEl.textContent = '⚠️ ' + (result.message || '此姓名已完成點名');
          msgEl.className = 'message duplicate';
        }
      } catch (err) {
        msgEl.style.display = 'block';
        msgEl.textContent = '❌ 發生錯誤，請稍後再試。';
        msgEl.className = 'message duplicate';
      }
    });
  </script>
</body>
</html>
