<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <title>学校選択</title>
</head>
<body>
  <label>地域:
    <select id="region">
      <option value="">選択してください</option>
      <option value="多治見市">多治見市</option>
      <option value="土岐市">土岐市</option>
      <option value="可児市">可児市</option>
    </select>
  </label>

  <br><br>

  <label>学校:
    <select id="school" disabled>
      <option value="">地域を選択してください</option>
    </select>
  </label>

  <br><br>

  <label>言語:
    <select id="language">
      <option value="">選択してください</option>
      <option value="日本語">日本語</option>
      <option value="英語">英語</option>
      <option value="ポルトガル語">ポルトガル語</option>
    </select>
  </label>

  <br><br>

  <button id="nextBtn" disabled>次へ</button>

  <script>
    const API_URL = '【ここにあなたのApps ScriptのURLを入れてください】';
    let data = [];

    const region = document.getElementById('region');
    const school = document.getElementById('school');
    const language = document.getElementById('language');
    const nextBtn = document.getElementById('nextBtn');

    fetch(API_URL)
      .then(res => res.json())
      .then(json => {
        data = json;
      });

    region.addEventListener('change', () => {
      const selectedRegion = region.value;
      school.innerHTML = '<option value="">選択してください</option>';
      if (selectedRegion) {
        const schools = [...new Set(data.filter(d => d['地域'] === selectedRegion).map(d => d['学校']))];
        schools.forEach(s => {
          const opt = document.createElement('option');
          opt.value = s;
          opt.textContent = s;
          school.appendChild(opt);
        });
        school.disabled = false;
      } else {
        school.disabled = true;
      }
      checkEnable();
    });

    school.addEventListener('change', checkEnable);
    language.addEventListener('change', checkEnable);

    function checkEnable() {
      if (school.value && language.value) {
        nextBtn.disabled = false;
      } else {
        nextBtn.disabled = true;
      }
    }

    nextBtn.addEventListener('click', () => {
      const selected = data.find(d =>
        d['地域'] === region.value &&
        d['学校'] === school.value &&
        d['言語'] === language.value
      );
      if (selected) {
        window.location.href = selected['URL'];
      } else {
        alert('リンク先が見つかりません。');
      }
    });
  </script>
</body>
</html>
